---
title: "올리브영 이미지에서 텍스트 추출하기"
layout: post
categories: "teamProject 화장품마케팅분석"
---

## 화장품마다 광고 키워드를 가져오기 위해 광고 사진에서 텍스트를 추출했다

**과정**

tesseract를 사용했다가 영어는 괜찮은데 한글이 너무 추출이 안 되고 깨졌다.  
우린 한국에서의 마케팅 분석이기 때문에 영어보다 한글이 중요한 만큼 tesseract를 포기하고
다른 방법을 찾아봤다.

다음으로는 구글의 cloud vision api를 사용해봤다.  
한글도 문제없이 잘 추출됐다. 하지만 안되는 이미지들이 엄청 많았다. 찾아보니까 다른 사람들도
겪고 있는 문제였지만 해결됐다는 말이 없었다.

그래서 다음으로는 이미지 추출 사이트에서 동적으로 돌리고 추출된 텍스트를 가져오는 코드로 바꿨다.
이 방법은 사실상 완성된 다른 사이트를 이용하는거여서  
대부분 문제없이 돌아갔지만 용량이 큰 몇몇 gif들은 프리미엄을 사라고 나왔다.
안 되는 gif들만 확인해보니까 중요한 텍스트가 따로 없어서 안 되는 것들은 버렸다.
이번에는 최종 방법에 대해서만 올리겠다.

**저장했던 데이터 불러오기**

저장해놓은 데이터 불러오기

```python
# CSV 파일 읽기
img_df = pd.read_csv('..//올리브영 제품 링크//oliveyoung_img_link.csv')


# 이미지 링크 열 이름 수정 ('img_link'로 가정)
image_links1 = img_df['img_link'].tolist()

# URL 확인 및 정리 - 리스트 요소를 문자열로 변환
image_links2 = [url.strip("[]") for url in image_links1 if isinstance(url, str)]
image_links2 = [url.replace("\'","") for url in image_links2 if isinstance(url, str)]

links=[[] for i in range(len(image_links2))]

for i in range(len(image_links2)):
    for j in image_links2[i].split(','):
        j=j.strip()
        links[i].append(j)
```

사이트 추출해서 최종으로 저장하는 코드

```python
    def text_get(x,y):
    # if links[x][y][-3:]=='gif':
    #     return
    driver.get(links[x][y])
    links[x][y]=driver.current_url
    driver.back()
    driver.find_element(By.CLASS_NAME, 'enterUrl ').click()
    driver.execute_script("window.scrollTo(0,500)")
    time.sleep(0.5)
    try:
        driver.find_element(By.CSS_SELECTOR, "#uploadfile > div > div.col-xl-9 > div.col-12.image_back.toolColor.text-center.m-0-auto.d-block.br_10.p-3 > div > div.col-12.urlArea.d_none.mt-3 > div > input").send_keys(links[x][y])
        driver.find_element(By.CLASS_NAME, 'urlBtn ').click()
        driver.execute_script("window.scrollTo(0,800)")
        time.sleep(1)
        clickable_element = WebDriverWait(driver, 10).until(
            EC.element_to_be_clickable((By.CLASS_NAME, 'convertFiles'))
        )
        time.sleep(10)
        clickable_element.click()
        # driver.find_element(By.CLASS_NAME, 'convertFiles').click()
        driver.execute_script("window.scrollTo(0,700)")
        #가능해질 때까지 기다리기
        clickable_element = WebDriverWait(driver, 10).until(
            EC.element_to_be_clickable((By.CLASS_NAME, 'copyData'))
        )
        time.sleep(25)
        clickable_element.click()
        driver.execute_script("window.open('{}')".format('https://n.lrl.kr/'))
        driver.switch_to.window(driver.window_handles[1])
        # driver.find_element(By.CLASS_NAME, 'copyData').click()
        time.sleep(1)
        a=driver.find_element(By.CLASS_NAME, 'note-editable')
        a.click()
        a.send_keys(Keys.CONTROL, 'v')
        time.sleep(1)
        html=driver.page_source
        soup=BS(html,'html.parser')
        result=soup.find("div",attrs={"class":"note-editable"})
        if result.text!='':
            if len(text[x])>=y:
                text[x][y]=result.text
            else:
                text[x].append(result.text)
    except Exception as e:
        print(str(x)+"-"+str(y))
        print(e)
    finally:
        driver.close()
        time.sleep(1)

        driver.switch_to.window(driver.window_handles[0])
        driver.execute_script("window.scrollTo(0,1200)")
        time.sleep(1)
        driver.find_element(By.CLASS_NAME, 're_convert').click()

driver = webdriver.Chrome()
driver.get('https://www.cardscanner.co/ko/image-to-text')
for i in re_index:
    text_get(int(i.split("-")[0]),int(i.split("-")[1]))

raw_data = pd.DataFrame()
raw_data['text'] = plus_text
raw_data.to_csv("oliveyoung_text.csv")
```
