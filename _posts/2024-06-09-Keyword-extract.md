---
title: "텍스트 키워드 추출"
layout: post
categories: "팀프로젝트"
---

## 저번에 추출해온 텍스트에서 키워드를 추출했다

**과정**

키워드 빈도수 탑 100을 보고 필요없는 키워드들을 불용어 처리 후 다시 탑 100을 봤다 점점 순위가
내려갈수록 필요없는 키워드들이 나오는 것을 보고 그 이후 키워드들을 다 자르고 53개의 키워드가 남았다.

**코드**

저장해놓은 데이터 불러오기

```python
# CSV 파일 읽기
olive = pd.read_csv('..//올리브영 데이터//oliveyoung_text.csv')
coupang = pd.read_csv('..//쿠팡 데이터//coupang_text.csv')

# 띄어쓰기 변환 함수
def correct_spacing(text):
    okt = Okt()
    tokens = okt.morphs(text)
    corrected_text = ' '.join(tokens)
    return corrected_text

#텍스트 형태소 분석하기 쉽게 처리
docs=olive
for i in range(len(docs)):
    docs['text'][i] = str(docs['text'][i]).replace("\r","")
    docs['text'][i] = str(docs['text'][i]).replace("\n"," ")
    docs['text'][i] = correct_spacing(docs['text'][i])
    docs['text'][i] = re.sub("[0-9]", '', str(docs['text'][i]))

#형태소 분석
title_token_list = [] # 제목의 형태소를 담아낼 리스트
title_token_noun = [] # 제목의 명사를 담아낼 리스트
for i in tqdm(range(len(docs))):
    # komoran.pos() 메서드를 사용하여 형태소 분석 실시
    try:
        pos = komoran.pos(u'{}'.format(docs['text'][i]))
    except Exception as e:
        print(i)
        print(e)
    # komoran.nouns() 메서드를 사용하여 길이가 2이상인 명사를 추출라고 리스트에 저장
    try:
        noun = list(term for term in komoran.nouns(u'{}'.format(docs['text'][i])) if len(term) >1)
    except Exception as e:
        print(i)
        print(e)
    title_token_list.append(pos) # 형태소 분석결과를 리스트에 추가
    title_token_noun.append(noun) # 추출한 명사를 리스트에 추가

#불용어 사전 불러오기
f = open("stopwords-ko.txt", "r", encoding="UTF-8")
st = f.readlines()
f.close()
stw = []
for i in range(0, len(st)):
    stw.append(st[i].rstrip('\n')) # st리스트에서 '\n' 제거

# 사용자가 불용어 추가
user_stopwords = ['크림', '아토', '베리','라마', '이드','피부', '크림',
                  '사용', '쇼핑', '이드','제품', '화장품','글리','도움',
                  '레이', '효과', '부위','라마', '폴리','경우','기능',
                  '뷰티', '로켓', '사항', '상품','씨드', '세린', '명품',
                  '라이', '관리', '주의', '용량', '원료', '광선', '유지',
                  '알코올', '공급', '상담', '완료', '기준','아크릴', '적용',
                  '스킨', '개월', '알란', '프릴', '소비자', '보관', '필요',
                  '로에베', '티코', '다이올', '외부','제수','고민','인체',
                  '사이드','특성', '얼굴', '발라', '이나', '에센스', '아마이드',
                  '폴리머', '오스', '당량', '해결', '에탄올','기한', '에이트',
                  '취급', '증상','상처','개인','결과', '올리브', '만족', '도포',
                  '부문','센터', '라스트', '판매', '성인', '브랜드', '연구원', '베리',
                  '기술', '원료', '마스크', '기획','기간', '리프', '전체', '확인', '워드',
                  '비교', '데카', '파트', '코스', '기관', '반응', '청규', '아토','무도','설문',
                  '토너','랭킹','온라인', '김정문', '수다','라보', '지오', '증가', '방법','페이',
                    '집중', '물질', '리얼','직후','세계','경과','증정','이미지', '페이스',
                  '롤라이','대비','키스', '카프', '증정', '티에', '일리', '반점', '녹차', '가지',
                  '고시', '방법', '자제', '어린이', '이지','고객', '밸런스', '베타', '트리', '공정',
                  '제조', '책임', '에도', '왁스', '토인', '레시틴', '거래', '위원회', '분쟁', '판매업',
                  '프로', '나무', '다이', '번호', '충전', '글로리', '여과', '페이', '보상', '형성', '리페', '에이치','비탄']

stw.extend(user_stopwords)
import csv
with open('불용어.csv','w') as file :
    write = csv.writer(file)
    write.writerow(stw)

#불용어 제거
for word in stw:
    for i in range(0, len(title_token_noun)):
        # 리스트에 불용어가 있을 경우 제거
        try:
            while word in title_token_noun[i]:
                title_token_noun[i].remove(word)
        except:
            pass

# 키워드 저장
raw_data = pd.DataFrame()
raw_data['keyword'] = title_token_noun

raw_data.to_csv("oliveyoung_keyword.csv")

#키워드 빈도수 탑
noun = list(itertools.chain(*title_token_noun)) # 리스트 접합
count = Counter(noun)
top = dict(count.most_common(53))
keyword=list(top.keys())

#최종 키워드 저장
raw_data = pd.DataFrame()
raw_data['top53'] = top
raw_data.to_csv("oliveyoung_top.csv")

#제품에 키워드가 최소 하나 이상 들어가있는지 확인하기
for i in range(len(olive)):
    for j in keyword:
        if olive['text'][i].find(j)!=-1:
            break
        else:
            if keyword=='저분자':
                print(i)
```
