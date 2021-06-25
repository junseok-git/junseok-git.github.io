\---
layout: post
title: "Traffic Flow Theory"
date: 2021-06-15 10:45:00 +0900
categories: jekyll update
\---



# 교통류이론 과목의 프로젝트 시 작성한 코드입니다.



___



## 1. 교차로 현시 분석 (경찰청측 API 서버 오류로 일부 교차로만 가능)


```python
import pandas as pd
import requests
from pandas.io.json import json_normalize
```


```python
# API 호출 키 설정 (key값은 개인정보..?)

url = 'http://apis.data.go.kr/1320000/CrossRoadInfoService/getCrossRoadInfoDetail?serviceKey='
key = 'DipZEtpF0sPuu%2FwtVKGjKRNI0qiMjpX76MVhhKytjPeN2HTXwLt1Ge4gFe1cqKQ91nkPfnRFwp0BKMKVTRvVQg%3D%3D'
params = '&type=json&srchCTId=L01' #srchCRNm=신당역

api = url+key+params
```


```python
# json 불러오기

response = requests.get(api)
json = response.json()
json[0]
```




    {'resultMsg': 'NORMAL_SERVICE',
     'totPage': '47',
     'totCnt': '470',
     'pageNo': '1',
     'resultCode': '0',
     'numOfRows': '100'}



* 총 교차로 개수가 470개만 불러와지는 오류 발생


```python
# 데이터프레임으로 변환하여 쉽게 확인 가능.

data = pd.json_normalize(json[1:])
data
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>REGION_CD</th>
      <th>A_RING_6_PHASE_CONF_CD</th>
      <th>INT_MAINPHASE</th>
      <th>A_RING_7_PHASE_CONF_CD</th>
      <th>B_RING_4_PHASE_CONF_CD</th>
      <th>INT_NO</th>
      <th>A_RING_4_PHASE_CONF_CD</th>
      <th>A_RING_8_PHASE_CONF_CD</th>
      <th>A_RING_1_PHASE_CONF_CD</th>
      <th>B_RING_8_PHASE_CONF_CD</th>
      <th>...</th>
      <th>B_RING_5_PHASE_CONF_CD</th>
      <th>B_RING_1_PHASE_CONF_CD</th>
      <th>INT_NM</th>
      <th>A_RING_3_PHASE_CONF_CD</th>
      <th>B_RING_2_PHASE_CONF_CD</th>
      <th>MAP_NO</th>
      <th>B_RING_3_PHASE_CONF_CD</th>
      <th>B_RING_7_PHASE_CONF_CD</th>
      <th>A_RING_2_PHASE_CONF_CD</th>
      <th>B_RING_6_PHASE_CONF_CD</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>L262352</td>
      <td>42</td>
      <td>L262352</td>
      <td>None</td>
      <td>S173354</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S353173</td>
      <td>영동대교남단</td>
      <td>S173354</td>
      <td>S353173</td>
      <td>0</td>
      <td>L172262</td>
      <td>None</td>
      <td>S173354</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>None</td>
      <td>43</td>
      <td>None</td>
      <td>None</td>
      <td>S082260</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S262080</td>
      <td>원앙예식장</td>
      <td>None</td>
      <td>P352</td>
      <td>0</td>
      <td>None</td>
      <td>None</td>
      <td>P352</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>L156259</td>
      <td>44</td>
      <td>L335082</td>
      <td>None</td>
      <td>S079260</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S259080</td>
      <td>청담사거리</td>
      <td>S160340</td>
      <td>L080171</td>
      <td>0</td>
      <td>S340161</td>
      <td>None</td>
      <td>L260351</td>
      <td>None</td>
    </tr>
    <tr>
      <th>3</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>L170262</td>
      <td>44</td>
      <td>L350082</td>
      <td>None</td>
      <td>S080260</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S260081</td>
      <td>청담사거리</td>
      <td>S170351</td>
      <td>L080172</td>
      <td>1</td>
      <td>S350171</td>
      <td>None</td>
      <td>L260352</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>L170262</td>
      <td>44</td>
      <td>L350082</td>
      <td>None</td>
      <td>S080260</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S260081</td>
      <td>청담사거리</td>
      <td>S170351</td>
      <td>L080172</td>
      <td>2</td>
      <td>S350171</td>
      <td>None</td>
      <td>L260352</td>
      <td>None</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>None</td>
      <td>2900</td>
      <td>None</td>
      <td>None</td>
      <td>S201021</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S021201</td>
      <td>숭례문</td>
      <td>S111291</td>
      <td>S021201</td>
      <td>1</td>
      <td>L110202</td>
      <td>None</td>
      <td>S180001</td>
      <td>None</td>
    </tr>
    <tr>
      <th>96</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>None</td>
      <td>3100</td>
      <td>None</td>
      <td>None</td>
      <td>S098277</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S278097</td>
      <td>봉천역</td>
      <td>None</td>
      <td>P007</td>
      <td>0</td>
      <td>None</td>
      <td>None</td>
      <td>P008</td>
      <td>None</td>
    </tr>
    <tr>
      <th>97</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>None</td>
      <td>3200</td>
      <td>None</td>
      <td>None</td>
      <td>S198016</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S018196</td>
      <td>둔촌동역</td>
      <td>S108286</td>
      <td>S288106</td>
      <td>0</td>
      <td>L108196</td>
      <td>None</td>
      <td>L288016</td>
      <td>None</td>
    </tr>
    <tr>
      <th>98</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>None</td>
      <td>3200</td>
      <td>None</td>
      <td>None</td>
      <td>S198016</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S018196</td>
      <td>둔촌동역</td>
      <td>S108286</td>
      <td>S288106</td>
      <td>1</td>
      <td>L108196</td>
      <td>None</td>
      <td>L288016</td>
      <td>None</td>
    </tr>
    <tr>
      <th>99</th>
      <td>L01</td>
      <td>None</td>
      <td>1</td>
      <td>None</td>
      <td>P360</td>
      <td>6800</td>
      <td>P360</td>
      <td>None</td>
      <td>S090270</td>
      <td>None</td>
      <td>...</td>
      <td>None</td>
      <td>S270090</td>
      <td>연1)이대역</td>
      <td>S090270</td>
      <td>S270090</td>
      <td>0</td>
      <td>S270090</td>
      <td>None</td>
      <td>S090270</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 21 columns</p>




___

## 2. 검지 지점 위치 확인


```python
import pandas as pd
import folium
import requests
from pyproj import Proj, transform
from bs4 import BeautifulSoup
```


```python
# API 호출 키 설정 (key값은 비밀번호..?)

url = 'http://openapi.seoul.go.kr:8088/'
key = '526545526f6a73653638587957554e'
params = '/xml/SpotInfo/1/1000/'

api = url+key+params
```


```python
# XML 호출

req = requests.get(api)
html = req.text
soup = BeautifulSoup(html, 'html.parser')
soup
```




    <?xml version="1.0" encoding="UTF-8" standalone="yes"?><spotinfo><list_total_count>169</list_total_count><result><code>INFO-000</code><message>정상 처리되었습니다</message></result><row><spot_num>A-01</spot_num><spot_nm>성산로(금화터널)</spot_nm><grs80tm_x>195489</grs80tm_x><grs80tm_y>452136</grs80tm_y></row><row><spot_num>A-02</spot_num><spot_nm>사직로(사직터널)</spot_nm><grs80tm_x>196756.776106</grs80tm_x><grs80tm_y>452546.638644</grs80tm_y></row><row><spot_num>A-03</spot_num><spot_nm>자하문로(자하문터널)</spot_nm><grs80tm_x>197216.855046</grs80tm_x><grs80tm_y>454350.990432</grs80tm_y></row><row><spot_num>A-04</spot_num><spot_nm>대사관로(삼청터널)</spot_nm><grs80tm_x>198648.893154</grs80tm_x><grs80tm_y>455200.108465</grs80tm_y></row><row><spot_num>A-05</spot_num><spot_nm>율곡로(안국역)</spot_nm><grs80tm_x>198645.671347</grs80tm_x><grs80tm_y>452937.216603</grs80tm_y></row><row><spot_num>A-06</spot_num><spot_nm>창경궁로(서울여자대학교)</spot_nm><grs80tm_x>199825.89671</grs80tm_x><grs80tm_y>453668.322568</grs80tm_y></row><row><spot_num>A-07</spot_num><spot_nm>대학로(한국방송통신대학교)</spot_nm><grs80tm_x>200185.014636</grs80tm_x><grs80tm_y>453144.745427</grs80tm_y></row><row><spot_num>A-08</spot_num><spot_nm>종로(동묘앞역)</spot_nm><grs80tm_x>201515.783975</grs80tm_x><grs80tm_y>452624.900832</grs80tm_y></row><row><spot_num>A-09</spot_num><spot_nm>퇴계로(신당역)</spot_nm><grs80tm_x>201850.055284</grs80tm_x><grs80tm_y>451769.96985</grs80tm_y></row><row><spot_num>A-10</spot_num><spot_nm>동호로(장충체육관)</spot_nm><grs80tm_x>200633.487975</grs80tm_x><grs80tm_y>451010.051429</grs80tm_y></row><row><spot_num>A-11</spot_num><spot_nm>장충단로(장충단공원)</spot_nm><grs80tm_x>200410</grs80tm_x><grs80tm_y>450804</grs80tm_y></row><row><spot_num>A-12</spot_num><spot_nm>퇴계로(회현역)</spot_nm><grs80tm_x>197939.908023</grs80tm_x><grs80tm_y>450888.928346</grs80tm_y></row><row><spot_num>A-13</spot_num><spot_nm>세종대로(서울역)</spot_nm><grs80tm_x>197727.33407</grs80tm_x><grs80tm_y>451006.009692</grs80tm_y></row><row><spot_num>A-14</spot_num><spot_nm>새문안로(서울역사박물관)</spot_nm><grs80tm_x>197432.421007</grs80tm_x><grs80tm_y>452243.292862</grs80tm_y></row><row><spot_num>A-15</spot_num><spot_nm>종로(종로3가역)</spot_nm><grs80tm_x>199240.067506</grs80tm_x><grs80tm_y>452288.413612</grs80tm_y></row><row><spot_num>A-16</spot_num><spot_nm>서소문로(시청역)</spot_nm><grs80tm_x>197596.4694</grs80tm_x><grs80tm_y>451463.528762</grs80tm_y></row><row><spot_num>A-17</spot_num><spot_nm>세종대로(시청역2)</spot_nm><grs80tm_x>197983.404032</grs80tm_x><grs80tm_y>452006.746671</grs80tm_y></row><row><spot_num>A-18</spot_num><spot_nm>을지로(을지로3가역)</spot_nm><grs80tm_x>199025.306571</grs80tm_x><grs80tm_y>451837.790166</grs80tm_y></row><row><spot_num>A-19</spot_num><spot_nm>칠패로(숭례문)</spot_nm><grs80tm_x>197579.482653</grs80tm_x><grs80tm_y>451141.212855</grs80tm_y></row><row><spot_num>A-20</spot_num><spot_nm>남산1호터널</spot_nm><grs80tm_x>200375.908853</grs80tm_x><grs80tm_y>448730.787276</grs80tm_y></row><row><spot_num>A-21</spot_num><spot_nm>남산2호터널</spot_nm><grs80tm_x>199098.824842</grs80tm_x><grs80tm_y>449434.634668</grs80tm_y></row><row><spot_num>A-22</spot_num><spot_nm>남산3호터널</spot_nm><grs80tm_x>198985.638133</grs80tm_x><grs80tm_y>449446.700998</grs80tm_y></row><row><spot_num>A-23</spot_num><spot_nm>소월로(회현역)</spot_nm><grs80tm_x>197913</grs80tm_x><grs80tm_y>450842</grs80tm_y></row><row><spot_num>A-24</spot_num><spot_nm>소파로(숭의여자대학교)</spot_nm><grs80tm_x>198564</grs80tm_x><grs80tm_y>450618</grs80tm_y></row></spotinfo>




```python
# XML -> 데이터프레임 변환

spot_num = []
spot_nm = []
x = []
y = []

spot_num_tmp = soup.find_all("spot_num")
spot_nm_tmp = soup.find_all("spot_nm")
x_tmp = soup.find_all("grs80tm_x")
y_tmp = soup.find_all("grs80tm_y")
    
for code in spot_num_tmp:
    spot_num.append(code.text)
for code in spot_nm_tmp:
    spot_nm.append(code.text)
for code in x_tmp:
    x.append(code.text)
for code in y_tmp:
    y.append(code.text)

df_spot = pd.DataFrame()
df_spot['spot_num'] = spot_num
df_spot['spot_nm'] = spot_nm
df_spot['x'] = x
df_spot['y'] = y

df_spot
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>spot_num</th>
      <th>spot_nm</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-01</td>
      <td>성산로(금화터널)</td>
      <td>195489</td>
      <td>452136</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A-02</td>
      <td>사직로(사직터널)</td>
      <td>196756.776106</td>
      <td>452546.638644</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A-03</td>
      <td>자하문로(자하문터널)</td>
      <td>197216.855046</td>
      <td>454350.990432</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A-04</td>
      <td>대사관로(삼청터널)</td>
      <td>198648.893154</td>
      <td>455200.108465</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A-05</td>
      <td>율곡로(안국역)</td>
      <td>198645.671347</td>
      <td>452937.216603</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>164</th>
      <td>F-06</td>
      <td>경부고속도로</td>
      <td>202107</td>
      <td>443264</td>
    </tr>
    <tr>
      <th>165</th>
      <td>F-07</td>
      <td>분당수서로</td>
      <td>207716</td>
      <td>444241</td>
    </tr>
    <tr>
      <th>166</th>
      <td>F-08</td>
      <td>강남순환로(관악터널)</td>
      <td>191832</td>
      <td>437667</td>
    </tr>
    <tr>
      <th>167</th>
      <td>F-09</td>
      <td>서부간선도로</td>
      <td>189560</td>
      <td>446828</td>
    </tr>
    <tr>
      <th>168</th>
      <td>Y-53</td>
      <td>잠실로(서울놀이마당앞~잠실트리지움)</td>
      <td>208208</td>
      <td>445659</td>
    </tr>
  </tbody>
</table>
<p>169 rows × 4 columns</p>





```python
# 검지지점 선택, 좌표변환

A_09 = df_spot[df_spot['spot_num'] == "A-09"]
A_09.reset_index(drop=True, inplace=True)

xy = transform(Proj(init='epsg:5181'), Proj(init='epsg:4326'), A_09['x'][0], A_09['y'][0])
A_09['lat'] = xy[1]
A_09['lng'] = xy[0]
A_09
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>spot_num</th>
      <th>spot_nm</th>
      <th>x</th>
      <th>y</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-09</td>
      <td>퇴계로(신당역)</td>
      <td>201850.055284</td>
      <td>451769.96985</td>
      <td>37.565464</td>
      <td>127.02094</td>
    </tr>
  </tbody>
</table>





```python
# 지도에 표시

m = folium.Map([A_09['lat'][0], A_09['lng'][0]], zoom_start=15)
folium.Marker([A_09['lat'][0], A_09['lng'][0]], popup=A_09['spot_nm'][0]).add_to(m)
m
```

![image-20210625205708893](image/2021-06-15-Traffic-Flow-Theory/image-20210625205708893.png)



___

## 3. 교통류 데이터 분석


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import os
import sys
import datetime as dt
```


```python
# 파일읽기

#path='C:/Users/USER/OneDrive - 서울시립대학교/학교용/4학년/01_교통류이론_한여희/과제/'  #노트북
path='D:/Junseok/OneDrive - UOS/학교용/4학년/01_교통류이론_한여희/과제/'  #집
tfd=pd.read_csv(path+'A-09.txt', skiprows=[0], encoding='cp949', quotechar="'", 
                names=['REALTIME_ID', 'LOCATION','INITLANE','AHEAD','COLLECT_DATE','vol','occ','spd'])
```


```python
tfd.head()
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>REALTIME_ID</th>
      <th>LOCATION</th>
      <th>INITLANE</th>
      <th>AHEAD</th>
      <th>COLLECT_DATE</th>
      <th>vol</th>
      <th>occ</th>
      <th>spd</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-09</td>
      <td>퇴계로(신당역)</td>
      <td>1</td>
      <td>유입</td>
      <td>2019-08-01 00:00:00</td>
      <td>4</td>
      <td>3.12</td>
      <td>37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A-09</td>
      <td>퇴계로(신당역)</td>
      <td>1</td>
      <td>유입</td>
      <td>2019-08-01 00:00:30</td>
      <td>3</td>
      <td>3.98</td>
      <td>27</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A-09</td>
      <td>퇴계로(신당역)</td>
      <td>1</td>
      <td>유입</td>
      <td>2019-08-01 00:01:00</td>
      <td>1</td>
      <td>1.85</td>
      <td>45</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A-09</td>
      <td>퇴계로(신당역)</td>
      <td>1</td>
      <td>유입</td>
      <td>2019-08-01 00:01:30</td>
      <td>3</td>
      <td>2.45</td>
      <td>46</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A-09</td>
      <td>퇴계로(신당역)</td>
      <td>1</td>
      <td>유입</td>
      <td>2019-08-01 00:02:00</td>
      <td>3</td>
      <td>4.04</td>
      <td>37</td>
    </tr>
  </tbody>
</table>





```python
# 기본 데이터프레임 변환

df=pd.DataFrame()

df['COLLECT_DATE'] = tfd.COLLECT_DATE
df['COLLECT_DATE'] = pd.to_datetime(df['COLLECT_DATE'])
df['DAY'] = df['COLLECT_DATE'].dt.weekday     # 요일 변환 (월:0 ~ 일:6)
df['DAY_n'] = df['COLLECT_DATE'].dt.day_name()
df['LOCATION'] = tfd.LOCATION.astype('str')
df['AHEAD'] = tfd.AHEAD.astype('str')
df['INITLANE'] = tfd.INITLANE.astype('int')
df['spd'] = tfd.spd.astype('float')
df['vol'] = tfd.vol.astype('int')
df.set_index(df['COLLECT_DATE'], inplace=True)
```


```python
df
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>COLLECT_DATE</th>
      <th>DAY</th>
      <th>DAY_n</th>
      <th>LOCATION</th>
      <th>AHEAD</th>
      <th>INITLANE</th>
      <th>spd</th>
      <th>vol</th>
    </tr>
    <tr>
      <th>COLLECT_DATE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-08-01 00:00:00</th>
      <td>2019-08-01 00:00:00</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>37.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2019-08-01 00:00:30</th>
      <td>2019-08-01 00:00:30</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>27.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2019-08-01 00:01:00</th>
      <td>2019-08-01 00:01:00</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>45.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-08-01 00:01:30</th>
      <td>2019-08-01 00:01:30</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>46.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2019-08-01 00:02:00</th>
      <td>2019-08-01 00:02:00</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>37.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2019-09-30 23:57:30</th>
      <td>2019-09-30 23:57:30</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>38.0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2019-09-30 23:58:00</th>
      <td>2019-09-30 23:58:00</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>32.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2019-09-30 23:58:30</th>
      <td>2019-09-30 23:58:30</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2019-09-30 23:59:00</th>
      <td>2019-09-30 23:59:00</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2019-09-30 23:59:30</th>
      <td>2019-09-30 23:59:30</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>56.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>855235 rows × 8 columns</p>





```python
# 무식하게 결측치 존재하는 날짜 및 시간대 찾기..

dfi1 = pd.DataFrame()
dfi2 = pd.DataFrame()
dfi3 = pd.DataFrame()
dfo1 = pd.DataFrame()
dfo2 = pd.DataFrame()
dfi = df[df['AHEAD'] == '유입']
dfi1 = dfi[dfi['INITLANE'] == 1]
dfi2 = dfi[dfi['INITLANE'] == 2]
dfi3 = dfi[dfi['INITLANE'] == 3]
dfo = df[df['AHEAD'] == '유출']
dfo1 = dfo[dfo['INITLANE'] == 1]
dfo2 = dfo[dfo['INITLANE'] == 2]
```


```python
# 무식하게 없는 날짜 찾기

na=pd.DataFrame()
nalist=[]
nalist=list(set(dfi.index) - set(dfo.index))
nalist.extend(list(set(dfo.index) - set(dfi.index)))
nalist.extend(list(set(dfi1.index) - set(dfi2.index)))
nalist.extend(list(set(dfi1.index) - set(dfi3.index)))
nalist.extend(list(set(dfi2.index) - set(dfi1.index)))
nalist.extend(list(set(dfi2.index) - set(dfi3.index)))
nalist.extend(list(set(dfi3.index) - set(dfi1.index)))
nalist.extend(list(set(dfi3.index) - set(dfi2.index)))
nalist.extend(list(set(dfo1.index) - set(dfo2.index)))
nalist.extend(list(set(dfo2.index) - set(dfo1.index)))
na['set']=nalist
na['set']=na['set'].astype('str')
na['set']=na['set'].apply(lambda x: x[:-9])
pd.Series.value_counts(na['set'])
```




    2019-08-20    285
    2019-09-03    186
    2019-09-05     38
    2019-09-04     20
    2019-09-16     14
    2019-09-14     11
    2019-09-13      2
    2019-08-29      2
    2019-08-05      2
    2019-09-20      1
    Name: set, dtype: int64




```python
print('제거 전 행 개수 =', len(df))
df = df[df.index.date != dt.date(2019,8,20)]  #목
df = df[df.index.date != dt.date(2019,9,3)]   #토
df
```

    제거 전 행 개수 = 855235




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>COLLECT_DATE</th>
      <th>DAY</th>
      <th>DAY_n</th>
      <th>LOCATION</th>
      <th>AHEAD</th>
      <th>INITLANE</th>
      <th>spd</th>
      <th>vol</th>
    </tr>
    <tr>
      <th>COLLECT_DATE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-08-01 00:00:00</th>
      <td>2019-08-01 00:00:00</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>37.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2019-08-01 00:00:30</th>
      <td>2019-08-01 00:00:30</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>27.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2019-08-01 00:01:00</th>
      <td>2019-08-01 00:01:00</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>45.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-08-01 00:01:30</th>
      <td>2019-08-01 00:01:30</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>46.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2019-08-01 00:02:00</th>
      <td>2019-08-01 00:02:00</td>
      <td>3</td>
      <td>Thursday</td>
      <td>퇴계로(신당역)</td>
      <td>유입</td>
      <td>1</td>
      <td>37.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2019-09-30 23:57:30</th>
      <td>2019-09-30 23:57:30</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>38.0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2019-09-30 23:58:00</th>
      <td>2019-09-30 23:58:00</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>32.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2019-09-30 23:58:30</th>
      <td>2019-09-30 23:58:30</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2019-09-30 23:59:00</th>
      <td>2019-09-30 23:59:00</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2019-09-30 23:59:30</th>
      <td>2019-09-30 23:59:30</td>
      <td>0</td>
      <td>Monday</td>
      <td>퇴계로(신당역)</td>
      <td>유출</td>
      <td>2</td>
      <td>56.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>827220 rows × 8 columns</p>





```python
# 요일 변동계수 계산

df_cv = df.groupby([df.index.date, 'DAY', 'DAY_n', 'AHEAD'], as_index=False)['vol'].sum()
df_cv = df_cv.groupby(['DAY', 'DAY_n', 'AHEAD'], as_index=False)['vol'].mean()

cv_tmp = df_cv.groupby(by=['AHEAD'], as_index=False)['vol'].mean()
cv_tmp = pd.DataFrame(cv_tmp)
cv_tmp.rename(columns = {'vol' : 'vol_A'}, inplace = True)

df_cv = pd.merge(df_cv, cv_tmp, how='outer', on='AHEAD')
df_cv['cv'] = df_cv['vol']/df_cv['vol_A']
df_cv.drop(columns=['vol_A'], inplace=True)

cv_tot = df.groupby([df.index.date, 'DAY', 'DAY_n'], as_index=False)['vol'].sum()
cv_tot = cv_tot.groupby(['DAY', 'DAY_n'], as_index=False)['vol'].mean()
cv_tot['cv'] = cv_tot['vol'] / cv_tot['vol'].mean()
cv_tot['AHEAD'] = '양방'

cv = pd.concat([df_cv, cv_tot])
cv.sort_values(by=['DAY', 'AHEAD'], inplace=True)
cv.reset_index(drop=True, inplace=True)
cv = cv.round(decimals=2)
cv
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DAY</th>
      <th>DAY_n</th>
      <th>AHEAD</th>
      <th>vol</th>
      <th>cv</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Monday</td>
      <td>양방</td>
      <td>43893.44</td>
      <td>1.01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Monday</td>
      <td>유입</td>
      <td>24260.11</td>
      <td>1.02</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Monday</td>
      <td>유출</td>
      <td>19633.33</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>Tuesday</td>
      <td>양방</td>
      <td>45482.17</td>
      <td>1.05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>Tuesday</td>
      <td>유입</td>
      <td>24791.50</td>
      <td>1.05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1</td>
      <td>Tuesday</td>
      <td>유출</td>
      <td>20690.67</td>
      <td>1.05</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>Wednesday</td>
      <td>양방</td>
      <td>45334.62</td>
      <td>1.05</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2</td>
      <td>Wednesday</td>
      <td>유입</td>
      <td>24670.00</td>
      <td>1.04</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2</td>
      <td>Wednesday</td>
      <td>유출</td>
      <td>20664.62</td>
      <td>1.05</td>
    </tr>
    <tr>
      <th>9</th>
      <td>3</td>
      <td>Thursday</td>
      <td>양방</td>
      <td>44325.33</td>
      <td>1.02</td>
    </tr>
    <tr>
      <th>10</th>
      <td>3</td>
      <td>Thursday</td>
      <td>유입</td>
      <td>24090.78</td>
      <td>1.02</td>
    </tr>
    <tr>
      <th>11</th>
      <td>3</td>
      <td>Thursday</td>
      <td>유출</td>
      <td>20234.56</td>
      <td>1.03</td>
    </tr>
    <tr>
      <th>12</th>
      <td>4</td>
      <td>Friday</td>
      <td>양방</td>
      <td>44137.88</td>
      <td>1.02</td>
    </tr>
    <tr>
      <th>13</th>
      <td>4</td>
      <td>Friday</td>
      <td>유입</td>
      <td>24093.62</td>
      <td>1.02</td>
    </tr>
    <tr>
      <th>14</th>
      <td>4</td>
      <td>Friday</td>
      <td>유출</td>
      <td>20044.25</td>
      <td>1.02</td>
    </tr>
    <tr>
      <th>15</th>
      <td>5</td>
      <td>Saturday</td>
      <td>양방</td>
      <td>42432.56</td>
      <td>0.98</td>
    </tr>
    <tr>
      <th>16</th>
      <td>5</td>
      <td>Saturday</td>
      <td>유입</td>
      <td>23137.89</td>
      <td>0.98</td>
    </tr>
    <tr>
      <th>17</th>
      <td>5</td>
      <td>Saturday</td>
      <td>유출</td>
      <td>19294.67</td>
      <td>0.98</td>
    </tr>
    <tr>
      <th>18</th>
      <td>6</td>
      <td>Sunday</td>
      <td>양방</td>
      <td>37569.67</td>
      <td>0.87</td>
    </tr>
    <tr>
      <th>19</th>
      <td>6</td>
      <td>Sunday</td>
      <td>유입</td>
      <td>20681.22</td>
      <td>0.87</td>
    </tr>
    <tr>
      <th>20</th>
      <td>6</td>
      <td>Sunday</td>
      <td>유출</td>
      <td>16888.44</td>
      <td>0.86</td>
    </tr>
  </tbody>
</table>





```python
# 전체 교통량 유입 : 유출 비율

print("전체 교통량 (유입 : 유출) 비율 =", cv_tmp['vol_A'][0] / cv_tmp['vol_A'][1])
cv_tmp
```

    전체 교통량 (유입 : 유출) 비율 = 1.2057073256350088





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AHEAD</th>
      <th>vol_A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>유입</td>
      <td>23675.017857</td>
    </tr>
    <tr>
      <th>1</th>
      <td>유출</td>
      <td>19635.791667</td>
    </tr>
  </tbody>
</table>





```python
# 데이터 처리 함수

df_result = pd.DataFrame()
TO = None
DAY = None

def df_sample(Tm):
    
    global df_result
    global TO
    global DAY
    
    Tm_tmp = 60/float(Tm[:-1])
    df_tmp = pd.DataFrame()
    df2 = df  # 원본데이터 보호용,,,
    
    if To == '유입':
        df2 = df2[df2['AHEAD'] == '유입']
        TO = 'IN'
    elif To == '유출':
        df2 = df2[df2['AHEAD'] == '유출']
        TO = 'OUT'
    elif To == '양방':
        TO = 'BOTH'
    else :
        print("To를 잘못 입력해서 양방향으로 진행합니다.\n유입, 유출, 양방 중에 고르셔용")
        TO = 'BOTH'
    
    
    if Day == '평일':
        df2 = df2[df2['DAY'] < 5]
        df2 = df2[df2.index.date != dt.date(2019,8,15)]  #휴일은 임의로 기입
        df2 = df2[df2.index.date != dt.date(2019,9,12)]
        df2 = df2[df2.index.date != dt.date(2019,9,13)]
        DAY = 'WEEKDAY'
    elif Day == '주말':
        df2 = df2[df2['DAY'] > 4]
        DAY = 'WEEKEND'
    elif Day == '일요일':
        df2 = df2[df2['DAY'] == 6]
        df2.append(df2[df2.index.date == dt.date(2019,8,15)])
        df2.append(df2[df2.index.date == dt.date(2019,9,12)])
        df2.append(df2[df2.index.date == dt.date(2019,9,13)])
        df2.append(df2[df2.index.date == dt.date(2019,9,14)])
        DAY = 'SUNDAY'
    elif Day == '전체':
        DAY = 'EVERYDAY'
    else :
        print("Day를 잘못 입력해서 전체로 진행합니다.\n전체, 평일, 주말, 일요일 중에 고르셔용")
        DAY = 'EVERYDAY'
    
    df2['spd_mult_vol'] = df2['spd']*df2['vol']
    df_tmp['SMS'] = df2.spd_mult_vol.resample(Tm).sum()/df2.vol.resample(Tm).sum()
    df_tmp['vol'] = df2.vol.resample(Tm).sum()*Tm_tmp/df2.INITLANE.max()
    df_tmp['den'] = df_tmp['vol']/df_tmp['SMS']
    df_tmp['COLLECT_DATE'] = df2.COLLECT_DATE.resample(Tm).min()
    df_tmp['Hour'] = df_tmp['COLLECT_DATE'].dt.hour
    df_tmp['AHEAD'] = TO
    df_tmp['DAY'] = DAY
    
    df_result = df_tmp

    return df_result
```


```python
# 분석 대상 입력받기

Day = input("분석 요일(평일, 주말, 일요일, 전체 중 입력) : ")

To = input("분석 방향(유입, 유출, 양방 중 입력) : ")

Tm = input("분석 시간 단위(분) : ")
Tm = Tm + 'T'

print("\n확인)", Day, To, Tm[:-1],"분")
```

    분석 요일(평일, 주말, 일요일, 전체 중 입력) : 평일
    분석 방향(유입, 유출, 양방 중 입력) : 유출
    분석 시간 단위(분) : 5
    
    확인) 평일 유출 5 분



```python
# 분석 모두 한번에 (지금 주석처리)

Dl = ['평일', '주말', '일요일', '전체']
Tol = ['유입', '유출', '양방']
Tml = ['1T', '2T', '5T', '15T']

for i in range(len(Dl)):
    Day = Dl[i]
    
    for j in range(len(Tol)):
        To = Tol[j]
        
        for z in range(len(Tml)):
            Tm = Tml[z]
            
            df_sample(Tm)
            Q_K(df_result)
            U_K(df_result)
```




```python
# PHF 계산 함수

df_phf_in = pd.DataFrame()
df_phf_out = pd.DataFrame()
df_phf_both = pd.DataFrame()
ph = pd.DataFrame()

def phf():
    global df_phf_in
    global df_phf_out
    global df_phf_both
    global ph
    
    df_phf = pd.DataFrame()
    df_phf['time'] = df_result.index
    df_phf['time'] = df_phf['time'].astype('str')
    df_phf['time'] = df_phf['time'].apply(lambda x: x[-9:])
    df_phf['time'] = df_phf['time'].apply(lambda x: x[:-3])
    df_phf['SMS'] = df_result.reset_index(drop=True)['SMS']
    df_phf['vol'] = df_result.reset_index(drop=True)['vol']
    df_phf['den'] = df_result.reset_index(drop=True)['den']
    df_phf = df_phf.groupby(['time'], as_index=False).mean()
    df_phf = df_phf.dropna(axis=0)
    
    if TO == 'IN':
        df_phf_in = df_phf
        df_phf_in['vol'] = df_phf_in['vol']*3  # 차로별 평균이므로 차로개수 곱하기
        df_phf_in['den'] = df_phf_in['den']*3  # 임시로 기입. 다른 구간일 경우 수정필요
    elif TO == 'OUT':
        df_phf_out = df_phf
        df_phf_out['vol'] = df_phf_out['vol']*2
        df_phf_out['den'] = df_phf_out['den']*2
    else:
        df_phf_both = df_phf
        df_phf_both['vol'] = df_phf_both['vol']*5
        df_phf_both['den'] = df_phf_both['den']*5

    peak = []
    peakhour = []
    for i in range(len(df_phf)-4):
        p = df_phf['vol'][i:i+4].sum()
        h = df_phf['time'][i]+" ~"+df_phf['time'][i+4]

        peak.append(p)
        peakhour.append(h)
    
    ph = pd.DataFrame()
    ph['hour'] = peakhour
    ph['vol'] = peak
    ph_vol = ph['vol'].max()
    #ph.sort_values(by='vol', ascending=False)   # 보여주기용
    
    
    # 피크시간대 15분 최대 교통량
    
    ph_i = ph['vol'].idxmax()
    p15_vol = df_phf['vol'][ph_i:ph_i+4].max()
    #df_phf[ph_i:ph_i+4]    # 보여주기용
    
    
    # PHF 계산

    PHF = ph_vol/(p15_vol*4)
    print("\n첨두시간교통량(", DAY, TO, ") =", format(ph_vol, ".2f"), 
          "\n첨두15분 교통량(", DAY, TO, ") =", format(p15_vol, ".2f"))
    print("PHF =", format(PHF, ".2f"))
```


```python
# 15분 단위 시간대별 교통량 그래프 함수

def phf_plt():
    plt.style.use("seaborn-whitegrid")
    plt.figure(figsize=(12,8))
    title = 'Hourly Vol ('+DAY+'_'+Tm[:-1]+'min)'
    plt.title(title, fontsize=30)
    plt.plot(df_phf_in['time'], df_phf_in['vol'], label='IN', c='blue')
    plt.plot(df_phf_out['time'], df_phf_out['vol'], label='OUT', c='green')
    plt.legend(loc='best', ncol=2, fontsize=14)
    x=df_phf_in['time']
    plt.xticks(ticks=x, rotation=45, fontsize=12)
    plt.yticks(fontsize=12)
    plt.xlabel('Hour', fontsize=20)
    plt.ylabel('Volume [vph]', fontsize=20)
    plt.locator_params(axis='x', nbins=len(x)/8)

    plt.axvline(x=df_phf_in['time'][df_phf_in['vol'].idxmax()], color='r', linestyle='--', linewidth=2)
    plt.axvline(x=df_phf_out['time'][df_phf_out['vol'].idxmax()], color='r', linestyle='--', linewidth=2)
```


```python
## PHF (15분 단위 교통량) 계산

Dl = ['평일', '일요일'] #, '주말', '일요일', '전체'
Tol = ['유입', '유출']  #, '양방'
Tm = '15T'

for i in range(len(Dl)):
    Day = Dl[i]
    for j in range(len(Tol)):
        To = Tol[j]
        df_sample(Tm)
        phf()
    phf_plt()
```


    첨두시간교통량( WEEKDAY IN ) = 3807.21 
    첨두15분 교통량( WEEKDAY IN ) = 995.54
    PHF = 0.96
    
    첨두시간교통량( WEEKDAY OUT ) = 2692.07 
    첨두15분 교통량( WEEKDAY OUT ) = 684.46
    PHF = 0.98
    
    첨두시간교통량( SUNDAY IN ) = 814.11 
    첨두15분 교통량( SUNDAY IN ) = 208.21
    PHF = 0.98
    
    첨두시간교통량( SUNDAY OUT ) = 682.74 
    첨두15분 교통량( SUNDAY OUT ) = 175.65
    PHF = 0.97



![png](output_30_1.png)



![png](output_30_2.png)


___

## 4. Fundamental Diagram 작성


```python
import datetime as dt
from matplotlib import dates
import seaborn as sns
```


```python
# Q-K FD 그리기 함수 (자동그림저장)

pltc = None
clr = ['grey','rosybrown','yellow','skyblue','orange','forestgreen','gold','midnightblue',
      'pink','lime','ivory','m','thistle','slategrey','magenta','lightcyan',
       'steelblue','chartreuse','hotpink','y','aquamarine','lightcoral','blue','sienna']

def Q_K(df_result):
    
    df_Hour = pd.DataFrame()
    global pltc
    
    if DAY == 'WEEKDAY':
        pltc = 'blue'
    elif DAY == 'WEEKEND':
        pltc = 'green'
    elif DAY == 'SUNDAY':
        pltc = 'red'
    elif DAY == 'EVERYDAY':
        pltc = 'orange'

    plt.style.use("seaborn-whitegrid")
    # Q-K
    title = 'Q-K FD ('+DAY+'_'+TO+'_'+Tm[:-1]+'min)'
    filename = 'plt/Q-K_'+DAY+'_'+TO+'_'+Tm[:-1]+'min.png'
    plt.figure(figsize=(12,8))
    plt.xlim(0,80)
    plt.ylim(0,800)
    plt.scatter(df_result['den'],df_result['vol'],alpha=0.4,s=10,marker='s',c=pltc)
    plt.title(title, fontsize=30)
    plt.xlabel('Density [vpk]', fontsize=20)
    plt.ylabel('Volume [vph]', fontsize=20)
    #plt.show()
    plt.savefig(path+filename, dpi=150)
    
    # 시간대별 Q-K 한 그림
    title = 'Q-K FD Hourly ('+DAY+'_'+TO+'_'+Tm[:-1]+'min)'
    filename = 'plt/Q-K_'+DAY+'_'+TO+'_'+Tm[:-1]+'min_Hourly.png'
    plt.figure(figsize=(12,8))
    plt.xlim(0,80)
    plt.ylim(0,800)
    for i in range(0,24):
        df_Hour=df_result[(df_result.Hour==i)]
        plt.scatter(df_Hour['den'],df_Hour['vol'],alpha=0.9,s=10,marker='s',c=clr[i])
    plt.title(title, fontsize=30)
    plt.legend(range(24))
    plt.xlabel('Density [vpk]', fontsize=20)
    plt.ylabel('Volume [vph]', fontsize=20)
    #plt.show()
    plt.savefig(path+filename, dpi=150)
    
    # 시간대별 Q-K 개별그림
    filename = 'plt/Q-K_'+DAY+'_'+TO+'_'+Tm[:-1]+'min_Each.png'
    plt.figure(figsize=(36,72))
    for i in range(0,24):
        df_Hour = df_result[(df_result.Hour==i)]
        plt.subplot(8,3,i+1)
        plt.xlim(0,80)
        plt.ylim(0,800)
        plt.scatter(df_Hour['den'],df_Hour['vol'],alpha=0.9,s=10,marker='s',c=pltc)
        plt.title('Q-K FD %dH ('%i+DAY+'_'+TO+'_'+Tm[:-1]+'min)', fontsize=30)
        plt.xlabel('Density [vpk]', fontsize=20)
        plt.ylabel('Volume [vph]', fontsize=20)
    #plt.show()
    plt.savefig(path+filename, dpi=50)
```


```python
# U-K FD 그리기 함수 (자동그림저장)

pltc = None

def U_K(df_result):
    
    df_Hour = pd.DataFrame()
    global pltc
    
    if DAY == 'WEEKDAY':
        pltc = 'blue'
    elif DAY == 'WEEKEND':
        pltc = 'green'
    elif DAY == 'SUNDAY':
        pltc = 'red'
    elif DAY == 'EVERYDAY':
        pltc = 'orange'

    plt.style.use("seaborn-whitegrid")
    # U-K
    title = 'U-K FD ('+DAY+'_'+TO+'_'+Tm[:-1]+'min)'
    filename = 'plt/U-K_'+DAY+'_'+TO+'_'+Tm[:-1]+'min.png'
    plt.figure(figsize=(12,8))
    plt.xlim(0,80)
    plt.ylim(0,80)
    plt.scatter(df_result['den'],df_result['SMS'],alpha=0.4,s=10,marker='s',c=pltc)
    plt.title(title, fontsize=30)
    plt.xlabel('Density [vpk]', fontsize=20)
    plt.ylabel('Speed [kph]', fontsize=20)
    #plt.show()
    plt.savefig(path+filename, dpi=150)
    
    # 시간대별 U-K 한 그림
    title = 'U-K FD Hourly ('+DAY+'_'+TO+'_'+Tm[:-1]+'min)'
    filename = 'plt/U-K_'+DAY+'_'+TO+'_'+Tm[:-1]+'min_Hourly.png'
    plt.figure(figsize=(12,8))
    plt.xlim(0,80)
    plt.ylim(0,80)
    for i in range(0,24):
        df_Hour=df_result[(df_result.Hour==i)]
        plt.scatter(df_Hour['den'],df_Hour['SMS'],alpha=0.9,s=10,marker='s', c=clr[i])
    plt.title(title, fontsize=30)
    plt.legend(range(24))
    plt.xlabel('Density [vpk]', fontsize=20)
    plt.ylabel('Speed [kph]', fontsize=20)
    #plt.show()
    plt.savefig(path+filename, dpi=150)
    
    # 시간대별 U-K 개별그림
    filename = 'plt/U-K_'+DAY+'_'+TO+'_'+Tm[:-1]+'min_Each.png'
    plt.figure(figsize=(36,72))
    for i in range(0,24):
        df_Hour = df_result[(df_result.Hour==i)]
        plt.subplot(8,3,i+1)
        plt.xlim(0,80)
        plt.ylim(0,80)
        plt.scatter(df_Hour['den'],df_Hour['SMS'],alpha=0.9,s=10,marker='s',c=pltc)
        plt.title('U-K FD %dH ('%i+DAY+'_'+TO+'_'+Tm[:-1]+'min)', fontsize=30)
        plt.xlabel('Density [vpk]', fontsize=20)
        plt.ylabel('Speed [kph]', fontsize=20)
    #plt.show()
    plt.savefig(path+filename, dpi=50)
```


```python
# Q-K, U-K 예시

Dl = ['평일', '일요일'] #, '주말', '일요일', '전체'
Tol = ['유입', '유출']  #, '양방'
Tm = '5T'

for i in range(len(Dl)):
    Day = Dl[i]
    
    for j in range(len(Tol)):
        To = Tol[j]
        
        df_sample(Tm)
        Q_K(df_result)
        U_K(df_result)
        #phf()
        #FD_line()
    #phf_plt()
```



![png](output_35_1.png)



![png](output_35_2.png)



![png](output_35_3.png)



![png](output_35_4.png)



![png](output_35_5.png)



![png](output_35_6.png)



![png](output_35_7.png)



![png](output_35_8.png)



![png](output_35_9.png)



![png](output_35_10.png)



![png](output_35_11.png)



![png](output_35_12.png)



![png](output_35_13.png)



![png](output_35_14.png)



![png](output_35_15.png)



![png](output_35_16.png)



![png](output_35_17.png)



![png](output_35_18.png)



![png](output_35_19.png)



![png](output_35_20.png)



![png](output_35_21.png)



![png](output_35_22.png)



![png](output_35_23.png)



![png](output_35_24.png)



```python
# 추세선 FD 함수

def FD_line():
    
    # Q-K
    sns.set_style("whitegrid")
    plt.figure(figsize=(12,8))
    title = 'Q-K FD ('+DAY+'_'+TO+'_'+Tm[:-1]+'min)'
    plt.xlim(0,80)
    plt.ylim(0,800)
    plt.title(title, fontsize=30)
    sns.regplot(x='den', y="vol", lowess=True, data=df_result,
               line_kws={'color':'red'}, scatter_kws={'color':'grey'})
    plt.xlabel('Density [vpk]', fontsize=20)
    plt.ylabel('Volume [vph]', fontsize=20)
    plt.show()
    
    # U-Q
    sns.set_style("whitegrid")
    plt.figure(figsize=(12,8))
    title = 'U-Q FD ('+DAY+'_'+TO+'_'+Tm[:-1]+'min)'
    plt.xlim(0,800)
    plt.ylim(0,80)
    plt.title(title, fontsize=30)
    sns.regplot(x='vol', y="SMS", lowess=True, data=df_result,
               line_kws={'color':'red'}, scatter_kws={'color':'grey'})
    plt.xlabel('Volume [vph]', fontsize=20)
    plt.ylabel('Speed [kph]', fontsize=20)
    plt.show()

    # U-K
    sns.set_style("whitegrid")
    plt.figure(figsize=(12,8))
    title = 'U-K FD ('+DAY+'_'+TO+'_'+Tm[:-1]+'min)'
    plt.xlim(0,80)
    plt.ylim(0,80)
    plt.title(title, fontsize=30)
    sns.regplot(x='den', y="SMS", lowess=True, data=df_result,
               line_kws={'color':'red'}, scatter_kws={'color':'grey'})
    plt.xlabel('Density [vpk]', fontsize=20)
    plt.ylabel('Speed [kph]', fontsize=20)
    plt.show()
```


```python
# 24시간 시간대별 평균 교통량, 추세선FD 그리기

Dl = ['평일', '일요일'] #, '주말', '일요일', '전체'
Tol = ['유입', '유출']  #, '양방'
Tm = '5T'

for i in range(len(Dl)):
    Day = Dl[i]
    
    for j in range(len(Tol)):
        To = Tol[j]
        
        df_sample(Tm)
        #Q_K(df_result)
        #U_K(df_result)
        #phf()
        FD_line()
    #phf_plt()
```


![png](output_37_0.png)



![png](output_37_1.png)



![png](output_37_2.png)



![png](output_37_3.png)



![png](output_37_4.png)



![png](output_37_5.png)



![png](output_37_6.png)



![png](output_37_7.png)



![png](output_37_8.png)



![png](output_37_9.png)



![png](output_37_10.png)



![png](output_37_11.png)



```python
# Triangular Diagram

Day = '평일'
To = '유입'
Tm = '5T'
df_sample(Tm)

def q_k_func(k):
    w=q_m/(k_j-k_c)
    return (k<k_c)*k*u_f + (k>=k_c)*-w*(k-k_j)

def u_k_func(k):
    z=-q_m/(k_j-k_c)
    return (k<=k_c)*u_f + (k>k_c)*z*(1-k_j/k)


k_c=12
k_j=60
q_m=650
u_f=q_m/k_c
k=np.arange(0.1,k_j)

plt.figure(figsize=(12,8))
plt.xlim(0,80)
plt.ylim(0,800)
plt.scatter(df_result['den'],df_result['vol'],alpha=0.4,s=10,marker='s')
plt.title('Q-K FD (WEEKDAY_IN_5min)', fontsize=20)
plt.xlabel('Density [vpk]', fontsize=14)
plt.ylabel('Volume [vph]', fontsize=14)

plt.plot(k,q_k_func(k),color="red",linestyle="solid")

plt.show()

plt.figure(figsize=(12,8))
plt.xlim(0,80)
plt.ylim(0,80)
plt.scatter(df_result['den'], df_result['SMS'],alpha=0.4,s=10,marker='s')
plt.title('U-K FD (WEEKDAY_IN_5min)', fontsize=20)
plt.xlabel('Density [vpk]', fontsize=14)
plt.ylabel('Speed [kph]', fontsize=14)

plt.plot(k,u_k_func(k),color="red",linestyle="solid")

plt.show()
   
```


![png](output_38_0.png)



![png](output_38_1.png)



