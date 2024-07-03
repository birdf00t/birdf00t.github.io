---
title: "올리브영 제품 리뷰 가져오기"
layout: post
categories: "teamProject 화장품마케팅분석"
---

## 올리브영 샘플 리뷰 가져오기

**소감**

리뷰가 url 차이도 없이 클릭으로만 나와서 동적 크롤ㄹ을 사용할 수 밖에 없었다.
올리브영 사이트 내에 제품이니까 다 양식이 같을 줄 알고 XPATH로 클릭을 했더니 몇몇 링크가
다른 양식이라서 다른 방법으로 바꿨다. 또 너무 오래 걸리다보니까 켜놓고 잠들었다가
화면이 꺼져서 날라가서 다시 처음부터 여러번 시작했다. 스크롤 내려가는 정도를
애매하게 해놨다가 넘어간 페이지도 여럿이었다. 마지막날엔 안 된 페이지만 다시 가져오려고 처리하다가
코드를 반대로 써서 필요한 11만개 데이터가 다 날라가고 필요없는 6개 데이터만 남기도 했다.
진짜 저때는 세상에서 사라지고 싶었다 저장하고 했어야 하는데 너무 안일했다
4일동안 제대로 못 자고 자다가 몇 시간 간격으로 일어나서 확인했는데 오히려 잠을 제대로 못 자느라 머리가 잘 안 돌아가서 4일이나 걸린 것 같기도 하다

리뷰가 다 있는줄 알았는데 올리브영에서 제품당 1000개만 보이게 해놔서
약 11만개의 리뷰를 가져왔다. 1000개만이라서 차라리 다행인 것 같기도ㅎ..

**샘플 리뷰 데이터 가져오기**

저장해놓은 데이터 불러오기

```python
#샘플 제품 링크
link = pd.read_csv('올리브영 제품 링크//oliveyoung_sample_link.csv')
link=link['link']
#그나마 저장해놨던 데이터
data=pd.read_csv('oliveyoung_reviews.csv')
```

데이터가 이상하게 저장되어 있어서 원래대로 분리 해주는 함수

```python
def str_out(st):
    return_text=[]
    t=""
    input_ok=0
    for i in st:
        if input_ok==1:
            if i=="\'":
                input_ok=0
                return_text.append(t)
                t=""
            else:
                t+=i
        else:
            if i=="\'":
                input_ok=1
    return return_text
```

데이터 세팅

```python
#별점
star=[]

for i in range(len(data['star'])):
    star.append(list(filter(lambda a: a.isdigit()==True,data['star'][i])))

star[26]=[]
#피부 타입
skin_type=[]
for i in range(len(data['skin_type'])):
    a=str_out(data['skin_type'][i])
    skin_type.append(a)
skin_type[26]=[]

#리뷰 내용
reviews=[]
for i in range(len(data['review'])):
    a=str_out(data['review'][i])
    reviews.append(a)
reviews[26]=[]
```

페이지 양식에 맞춰서 가져오는 함수

```python
def info_get(index):
    html = driver.page_source
    soup = BS(html, 'html.parser')

    info = soup.find_all("div", attrs={"class":"user clrfix"})
    for i in range(len(info)):
        #피부타입
        try:
            sk=info[i].find("p", attrs={"class":'tag'})
            type=""
            for k in sk.find_all("span"):
                type+=k.text+","

            type=type[:-1]
            skin_type[index].append(type)
        except:
            skin_type[index].append("")

    review = soup.find_all("div", attrs={"class":"review_cont"})
    for i in range(len(review)):
        #리뷰내용
        try:
            re=review[i].find("div", attrs={"class": "txt_inner"}).text
            reviews[index].append(re)
        except:
            reviews[index].append("")
         #별점
        try:
            st=review[i].find("span", attrs={ "class":"point"}).text
            st=st.split("5점만점에 ",1)[1]
            star[index].append(st)
        except:
            star[index].append("")
```

모든 링크 리뷰 가져오기

```python
driver = webdriver.Chrome()
driver.get('https://www.oliveyoung.co.kr/store/display/getMCategoryList.do?dispCatNo=100000100010015&isLoginCnt=0&aShowCnt=0&bShowCnt=0&cShowCnt=0&gateCd=Drawer&trackingCd=Cat100000100010015_MID&trackingCd=Cat100000100010015_MID&t_page=드로우_카테고리&t_click=카테고리탭_중카테고리&t_2nd_category_type=중_크림')

for index in range(26, len(star)):#check_index:
    page_num=1
    url = link[index]
    try:
        driver.switch_to.window(driver.window_handles[0]) # 첫 번째 탭으로 이동
        driver.execute_script("window.open('{}')".format(url)) # URL 실행`
        driver.switch_to.window(driver.window_handles[1]) # 두 번째 탭으로 이동
        time.sleep(1)
        driver.execute_script("window.scrollTo(0, 1500)")
        driver.find_element(By.ID, 'reviewInfo').click()
        time.sleep(1)
        driver.find_element(By.ID, 'searchType_1').click()
        driver.find_element(By.ID, 'searchType_3').click()
        driver.find_element(By.XPATH, '//*[@id="gdasSort"]/li[3]/a').click()
        time.sleep(1)
        driver.execute_script("window.scrollTo(0, 2500)")
        time.sleep(1)

        while True:
            try:
                time.sleep(1)
                driver.execute_script("window.scrollTo(0, 4000)")
                time.sleep(1.5)
                err=0
                for i in range(10):
                    if page_num==100:
                        info_get(index)
                        break
                    if page_num>10:
                        i+=1
                    page_num+=1
                    try:
                        info_get(index)
                        driver.execute_script("window.scrollTo(0, 4000)")
                        time.sleep(1)
                        page=driver.find_element(By.CLASS_NAME, 'pageing')
                        page=page.find_elements(By.TAG_NAME, 'a')
                        time.sleep(1)
                        page[i].click()
                        time.sleep(1)
                    except Exception as e:
                        print(e)
                        err=1
                        break
                if page_num==100:
                    break
                if err==1:
                    break
            except Exception as e:
                print(e)
                break
    except Exception as e:
        print("페이지 이상: link "+str(index))
    finally:
        driver.close()
        time.sleep(0.3)
        if(index%10==0):
            print(str(index)+"/"+str(len(link)))
```

데이터 제대로 못 가져온 링크 인덱스 따로 저장

```python
check_index=[]

for i in range(len(star)):
    if len(skin_type[i])==len(star[i])==len(reviews[i])==1000:
        print(reviews[i])
    else:
        skin_type[i]=[]#None
        star[i]=[]#None
        reviews[i]=[]#None
        check_index.append(i)
```
