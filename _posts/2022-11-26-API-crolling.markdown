---
title:  "공공데이터 포털 API 크롤링"
tag: 
  - 공공데이터 포털
  - 크롤링
  - Python
---

## 데이터 자료
<a href="https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15057511">국토교통부_아파트매매 실거래 상세 자료</a>
- 데이터 포맷 : XML
- API : REST

## 크롤링 코드

### API 연결
```
import requests

url = 'http://openapi.molit.go.kr/OpenAPI_ToolInstallPackage/service/rest/RTMSOBJSvc/getRTMSDataSvcAptTradeDev'
params ={'serviceKey' : '[승인키 : 디코딩]', 'pageNo' : '1', 'numOfRows' : '10', 'LAWD_CD' : '11110', 'DEAL_YMD' : '202012' }
response = requests.get(url, params=params)
```
- pageNo : 페이지 넘버
- numofRows : 한페이지에 나오는 데이터의 수
- LAWD_CD : 시군구코드
- DEAL_YWD : 가져올 년도

<br>

### 전체 데이터 가져오기
```
content = response.text
xml_obj = bs4.BeautifulSoup(content,'lxml-xml')
rows = xml_obj.findAll('item') #item 기준으로 나누기
```
<br>
<img width="983" alt="image" src="https://user-images.githubusercontent.com/55444587/204082558-e40ee5b8-2a59-46aa-abcb-6727fc763173.png">
<br>

- 전체 데이터를 item 태그 기준으로 나누어 리스트에 저장

<br>

### 컬럼 따로 뽑기
- 데이터 하나만 이용하여 컬럼을 따로 분리
```
columns_list = rows[0].find_all()
```
<br>
<img width="367" alt="image" src="https://user-images.githubusercontent.com/55444587/204082578-bff63cbe-05bb-4bc2-82ac-15f1fd54b96e.png">
<br>

- 반복문을 활용하여 columns 리스트에 컬럼 명을 넣는다.
```
columns = []
for columns_value in columns_list:
    columns.append(columns_value.name)
```
<br>
<img width="985" alt="image" src="https://user-images.githubusercontent.com/55444587/204082612-d66be993-b744-43f9-83d3-b56f1e205288.png">
<br>

### 데이터 추출하기
- date 라는 리스트안에 가져올 모든 데이터의 날짜를 넣는다.
- for 문을 통해서 params 에 순차적으로 넣어서 작업 진행
- rows 는 item 기준으로 진행이 되었기 때문에 len(rows)를 진행하게 되면 가져온 모든 데이터의 갯수를 확인 할 수 있다.
- 모든 데이터의 갯수 만큼 반복하여 순차적으로 한 행에 데이터를 넣고 데이터 프레임 제작을 위해 한번 더 리스트에 담는다 ( 2 차원 데이터 만들기)
```
full_value = [] # 전체 리스트
date = ['202001','202002','202003','202004','202005','202006','202007','202008','202009','202010','202011','202012',]
for i in date:
    url = 'http://openapi.molit.go.kr/OpenAPI_ToolInstallPackage/service/rest/RTMSOBJSvc/getRTMSDataSvcAptTradeDev'
    params ={'serviceKey' : 'muo3MlJMDnRxkpue7QeRh8XAOaU8pxYRXel6l7eyxr5H6H07EQyZbX7udurLMZdadPCEvkjf7Sg4RZmI84TRRg==', 'pageNo' : '1', 'numOfRows' : '100000', 'LAWD_CD' : '11110', 'DEAL_YMD' : f'{i}' }
    response = requests.get(url, params=params)
    content = response.text
    xml_obj = bs4.BeautifulSoup(content,'lxml-xml')
    rows = xml_obj.findAll('item') #item 기준으로 나누기
    length = len(rows) #전체 데이터 길이 구하기
    
    for num in range(length):
        columns_list = rows[num].find_all()
        value = [] # 한 행 값들
        for val in columns_list:
            value.append(val.text.lstrip())
        full_value.append(value)
```

<br>

### 데이터 프레임 만들기
- pd.DataFrame( [2차원 리스트], [columns 리스트] ) 형식으로 데이터 프레임을 제작
```
pd.DataFrame(full_value, columns=columns)
```
**full_value 값**

<br>
<img width="1083" alt="image" src="https://user-images.githubusercontent.com/55444587/204082649-a95b2966-997a-4038-9e82-0be0806b30a1.png">
<br>

**columns 값**

<br>
<img width="985" alt="image" src="https://user-images.githubusercontent.com/55444587/204082612-d66be993-b744-43f9-83d3-b56f1e205288.png">
<br>

### CSV 저장하기
- 위에세 만든 데이터 프레임을 utf-8-sig 형태로 추출
- 일반적인 utf-8으로는 한글 데이터가 깨짐으로 utf-8-sig 활용

```
test.to_csv("apt.csv", mode='w', encoding='utf-8-sig')
```
<br>
<img width="1410" alt="image" src="https://user-images.githubusercontent.com/55444587/204082673-197ebf15-6440-43cd-8e6d-52fb0b22086b.png">
<br>
