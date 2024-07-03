---
title: "올리브영 제품 링크 가져오기"
layout: post
categories: "teamProject 화장품마케팅분석"
---

## 화장품 키워드 마케팅 분석 전 올리브영에서 제품 링크 가져오기

**소감**

이번에는 데이터를 가져오기 전에 관련된 링크를 다 가져왔다.
시간에 쫓기느라 코드를 예쁘게 짜기보단 실행되기만 하면 된다라는 생각으로 짜서
나도 한 번에 이해하기 힘들 정도로 코드가 더럽다.

**모든 링크 가져오기**

크림 카테고리 제품이 총 27페이지였다
url에서 페이지 수를 바꿔가면서 데이터를 가져왔다

```python
driver = webdriver.Chrome()
driver.get('https://www.oliveyoung.co.kr/store/display/getMCategoryList.do?dispCatNo=100000100010015&isLoginCnt=0&aShowCnt=0&bShowCnt=0&cShowCnt=0&gateCd=Drawer&trackingCd=Cat100000100010015_MID&trackingCd=Cat100000100010015_MID&t_page=드로우_카테고리&t_click=카테고리탭_중카테고리&t_2nd_category_type=중_크림')

link=[]

for i in range(1,27):
    url ='https://www.oliveyoung.co.kr/store/display/getMCategoryList.do?dispCatNo=100000100010015&fltDispCatNo=&prdSort=01&pageIdx='+str(i)+'&rowsPerPage=24&searchTypeSort=btn_thumb&plusButtonFlag=N&isLoginCnt=0&aShowCnt=&bShowCnt=&cShowCnt=&trackingCd=Cat100000100010015_Small&amplitudePageGubun=&t_page=&t_click=&midCategory=%ED%81%AC%EB%A6%BC&smallCategory=%EC%A0%84%EC%B2%B4&checkBrnds=&lastChkBrnd='
    driver.switch_to.window(driver.window_handles[0]) # 첫 번째 탭으로 이동
    driver.execute_script("window.open('{}')".format(url)) # URL 실행
    driver.switch_to.window(driver.window_handles[1]) # 두 번째 탭으로 이동
    time.sleep(2)
    html = driver.page_source  # 현재 페이지의 HTML 코드를 가져옴
    soup = BS(html, 'html.parser')
    list = soup.find_all("div", attrs={"class": "prd_info"})
    for i in range(len(list)):
        a=list[i].find("a", attrs={"class":"prd_thumb goodsList"}).attrs['href']
        link.append(a)
    driver.close()
    time.sleep(0.3)
```

**리뷰가 2000개 이상인 제품만 링크 따로 저장하기**

아까 가져왔던 전체 제품 링크에 들어가서 리뷰 데이터가 2000개 이상이라면 링크를 저장했다 = 샘플 링크

```python
from selenium.common.exceptions import TimeoutException
review=[]

for i in range(len(raw_data)):
    url = raw_data['link'][i]
    try:
        driver.switch_to.window(driver.window_handles[0]) # 첫 번째 탭으로 이동
        driver.execute_script("window.open('{}')".format(url)) # URL 실행
        driver.switch_to.window(driver.window_handles[1]) # 두 번째 탭으로 이동
        time.sleep(0.5)
        html = driver.page_source  # 현재 페이지의 HTML 코드를 가져옴
        soup = BS(html, 'html.parser')
        review_count = soup.find("a", attrs={"class": "goods_reputation"})
        num=int(re.sub(r'[^0-9]', '', review_count.find("span").text))
        if (num>=2000):
            link.append(url)
            review.append(num)
    except TimeoutException as e:
        println(url)
    finally:
        driver.close()
        time.sleep(0.3)
        if(i%10==0):
            print(str(i)+"/"+str(len(raw_data)))
```

**리뷰가 2000개 이상인 제품의 광고 이미지 링크 가져오기**

샘플 링크에 들어가서 제품 설명 이미지를 다 가져왔다
제품마다 이미지가 한 개 인 곳도 여러개인 곳도 있어서 2차원 사용

```python
img_link=[[] for i in range(len(link))]
wo
for i in range(len(link)):
    url = link[i]
    try:
        driver.switch_to.window(driver.window_handles[0]) # 첫 번째 탭으로 이동
        driver.execute_script("window.open('{}')".format(url)) # URL 실행
        driver.switch_to.window(driver.window_handles[1]) # 두 번째 탭으로 이동
        time.sleep(0.5)
        html = driver.page_source  # 현재 페이지의 HTML 코드를 가져옴
        soup = BS(html, 'html.parser')
        driver.close()

        group = soup.find("div", attrs={"class": "iPrdViewimg"})
        if(group==None):
            group=soup.find_all("picture")
        img=[]
        for j in group:
            if isinstance(j, NavigableString):
                continue
            img.append(j.find_all("img"))
        for j in img:
            for m in j:
                if(m["src"][0:8]!="https://"):
                    img_link[i].append(m["data-src"])
                else:
                    img_link[i].append(m["src"])
    except Exception:
        println(url)
    finally:
        time.sleep(0.3)
        if(i%10==0):
            print(str(i)+"/"+str(len(link)))
```
