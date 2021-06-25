# 서울시립대 교통공학과 16학번 김준석 (2021년 1학기)

분석결과 및 의견은 함께 첨부한 발표자료(pdf)에 담았습니다.

# 교차로 현시 분석 (경찰청측 API 서버 오류로 일부 교차로만 가능)


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
response = requests.get(api)
json = response.json()
json
```




    [{'resultMsg': 'NORMAL_SERVICE',
      'totPage': '47',
      'totCnt': '470',
      'pageNo': '1',
      'resultCode': '0',
      'numOfRows': '100'},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L262352',
      'INT_NO': '42',
      'A_RING_4_PHASE_CONF_CD': 'L262352',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S173354',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S353173',
      'INT_NM': '영동대교남단',
      'A_RING_3_PHASE_CONF_CD': 'S173354',
      'B_RING_2_PHASE_CONF_CD': 'S353173',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L172262',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S173354',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '43',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S082260',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S262080',
      'INT_NM': '원앙예식장',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P352',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P352',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L156259',
      'INT_NO': '44',
      'A_RING_4_PHASE_CONF_CD': 'L335082',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S079260',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S259080',
      'INT_NM': '청담사거리',
      'A_RING_3_PHASE_CONF_CD': 'S160340',
      'B_RING_2_PHASE_CONF_CD': 'L080171',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S340161',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L260351',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L170262',
      'INT_NO': '44',
      'A_RING_4_PHASE_CONF_CD': 'L350082',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S080260',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S260081',
      'INT_NM': '청담사거리',
      'A_RING_3_PHASE_CONF_CD': 'S170351',
      'B_RING_2_PHASE_CONF_CD': 'L080172',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'S350171',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L260352',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L170262',
      'INT_NO': '44',
      'A_RING_4_PHASE_CONF_CD': 'L350082',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S080260',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S260081',
      'INT_NM': '청담사거리',
      'A_RING_3_PHASE_CONF_CD': 'S170351',
      'B_RING_2_PHASE_CONF_CD': 'L080172',
      'MAP_NO': '2',
      'B_RING_3_PHASE_CONF_CD': 'S350171',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L260352',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L082171',
      'INT_NO': '45',
      'A_RING_4_PHASE_CONF_CD': 'L262352',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S174353',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S354173',
      'INT_NM': '학동',
      'A_RING_3_PHASE_CONF_CD': 'S083263',
      'B_RING_2_PHASE_CONF_CD': 'L172261',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S263083',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L352081',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L069160',
      'INT_NO': '46',
      'A_RING_4_PHASE_CONF_CD': 'L249341',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S161341',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S341161',
      'INT_NM': '도산공원',
      'A_RING_3_PHASE_CONF_CD': 'S071251',
      'B_RING_2_PHASE_CONF_CD': 'L160250',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S251071',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L340070',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L069160',
      'INT_NO': '46',
      'A_RING_4_PHASE_CONF_CD': 'L249341',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S161341',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S341161',
      'INT_NM': '도산공원',
      'A_RING_3_PHASE_CONF_CD': 'S071251',
      'B_RING_2_PHASE_CONF_CD': 'L160250',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'S251071',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L340070',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '47',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S067247',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S247068',
      'INT_NM': '삼화콘덴서',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P337',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P337',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '48',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S063242',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S242062',
      'INT_NM': '영동호텔',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P333',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P333',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L072164',
      'INT_NO': '49',
      'A_RING_4_PHASE_CONF_CD': 'S072254',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'L342074',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S342164',
      'INT_NM': '신사역',
      'A_RING_3_PHASE_CONF_CD': 'S072254',
      'B_RING_2_PHASE_CONF_CD': 'S342164',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S252074',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S162344',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L069160',
      'INT_NO': '49',
      'A_RING_4_PHASE_CONF_CD': 'S071251',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'L339070',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S341161',
      'INT_NM': '신사역',
      'A_RING_3_PHASE_CONF_CD': 'S071251',
      'B_RING_2_PHASE_CONF_CD': 'S341161',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'S251071',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S161341',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S270090',
      'INT_NO': '65',
      'A_RING_4_PHASE_CONF_CD': 'S090270',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S180360',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': None,
      'INT_NM': '사당역',
      'A_RING_3_PHASE_CONF_CD': 'L270',
      'B_RING_2_PHASE_CONF_CD': 'L180270',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L090179',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L360090',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '66',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S198016',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S018196',
      'INT_NM': '둔촌종합상가',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P287',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L018106',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '66',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S198016',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S018196',
      'INT_NM': '둔촌종합상가',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P288',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L018106',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S270090',
      'INT_NO': '71',
      'A_RING_4_PHASE_CONF_CD': 'L270360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S180360',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': None,
      'INT_NM': '명일역',
      'A_RING_3_PHASE_CONF_CD': 'S090270',
      'B_RING_2_PHASE_CONF_CD': 'L180270',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L090180',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L360090',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S270090',
      'INT_NO': '71',
      'A_RING_4_PHASE_CONF_CD': 'L270360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S180360',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': None,
      'INT_NM': '명일역',
      'A_RING_3_PHASE_CONF_CD': 'S090270',
      'B_RING_2_PHASE_CONF_CD': 'L180270',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'L090180',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L360090',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L152243',
      'INT_NO': '74',
      'A_RING_4_PHASE_CONF_CD': 'L332063',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S062243',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S242063',
      'INT_NM': '길동프라자아파트',
      'A_RING_3_PHASE_CONF_CD': 'P332',
      'B_RING_2_PHASE_CONF_CD': 'L062153',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'P238',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L242333',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L181273',
      'INT_NO': '76',
      'A_RING_4_PHASE_CONF_CD': 'L001093',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S091271',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S271091',
      'INT_NM': '동부기술교육원',
      'A_RING_3_PHASE_CONF_CD': 'S181001',
      'B_RING_2_PHASE_CONF_CD': 'L091183',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S001181',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L271003',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '155',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S199020',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S019201',
      'INT_NM': '송정동공영주차장',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P294',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L019109',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '157',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S087267',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S267087',
      'INT_NM': '세운상가',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P356',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P356',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L074164',
      'INT_NO': '172',
      'A_RING_4_PHASE_CONF_CD': 'L254344',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S165344',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S345164',
      'INT_NM': '태릉입구역',
      'A_RING_3_PHASE_CONF_CD': 'S075254',
      'B_RING_2_PHASE_CONF_CD': 'L164254',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S255074',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L344074',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '173',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S109289',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S289109',
      'INT_NM': '엘웨딩부페',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P019',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P019',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S345164',
      'INT_NO': '207',
      'A_RING_4_PHASE_CONF_CD': 'S165344',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'L344073',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'L344073',
      'INT_NM': '롯데캐슬골드파크3차',
      'A_RING_3_PHASE_CONF_CD': 'S165344',
      'B_RING_2_PHASE_CONF_CD': 'L254344',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L164254',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L254344',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S259078',
      'INT_NO': '239',
      'A_RING_4_PHASE_CONF_CD': 'L259348',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S168348',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S348169',
      'INT_NM': '노원롯데백화점',
      'A_RING_3_PHASE_CONF_CD': 'S079258',
      'B_RING_2_PHASE_CONF_CD': 'L169258',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L078168',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L348078',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S328148',
      'INT_NO': '393',
      'A_RING_4_PHASE_CONF_CD': 'L329090',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S086267',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S267086',
      'INT_NM': '보훈병원입구',
      'A_RING_3_PHASE_CONF_CD': 'S147327',
      'B_RING_2_PHASE_CONF_CD': 'S267086',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L150272',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S086267',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '449',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S160342',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S340162',
      'INT_NM': '영동시장앞',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P249',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P248',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L072163',
      'INT_NO': '450',
      'A_RING_4_PHASE_CONF_CD': 'P251',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S162343',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S342163',
      'INT_NM': '박미삼거리',
      'A_RING_3_PHASE_CONF_CD': 'L342073',
      'B_RING_2_PHASE_CONF_CD': 'S342163',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L342073',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S162343',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '500',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'L172262',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'L172262',
      'INT_NM': '서울시교육연수원',
      'A_RING_3_PHASE_CONF_CD': 'S082263',
      'B_RING_2_PHASE_CONF_CD': 'L082172',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S262083',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S082263',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '553',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S160342',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S340162',
      'INT_NM': '독산동골프장',
      'A_RING_3_PHASE_CONF_CD': 'P250',
      'B_RING_2_PHASE_CONF_CD': 'S340162',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'P250',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S160342',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '554',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S118298',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S298118',
      'INT_NM': '방이119안전센터',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'S298118',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S118298',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '623',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S112292',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S292112',
      'INT_NM': '군자교',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'L202292',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L022112',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '631',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S162344',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S342164',
      'INT_NM': '신한은행역삼',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P249',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P250',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '632',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S150329',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S330149',
      'INT_NM': '서부여성발전센터',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P232',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P233',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '636',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S134313',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S314132',
      'INT_NM': '답십리삼거리',
      'A_RING_3_PHASE_CONF_CD': 'P044',
      'B_RING_2_PHASE_CONF_CD': 'S314132',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L199312',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S134313',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L180300',
      'INT_NO': '637',
      'A_RING_4_PHASE_CONF_CD': 'S180360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S090270',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S270090',
      'INT_NM': '신설동역앞',
      'A_RING_3_PHASE_CONF_CD': 'L360090',
      'B_RING_2_PHASE_CONF_CD': 'S270090',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S360180',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S090270',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L180297',
      'INT_NO': '637',
      'A_RING_4_PHASE_CONF_CD': 'S180360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S090270',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S270090',
      'INT_NM': '신설동역앞',
      'A_RING_3_PHASE_CONF_CD': 'L360090',
      'B_RING_2_PHASE_CONF_CD': 'S270090',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'S360180',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S090270',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '638',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S099278',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S279098',
      'INT_NM': '용답동우체국',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P009',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P009',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '639',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S113294',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S294114',
      'INT_NM': '군자교동측',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P024',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P023',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '641',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S161340',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S340160',
      'INT_NM': '아차산역',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P070',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P070',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '744',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S198017',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S018198',
      'INT_NM': '길동초교',
      'A_RING_3_PHASE_CONF_CD': 'P286',
      'B_RING_2_PHASE_CONF_CD': 'L199287',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'P284',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S198017',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L189279',
      'INT_NO': '758',
      'A_RING_4_PHASE_CONF_CD': 'S189008',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S077256',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S257076',
      'INT_NM': '구로전화국',
      'A_RING_3_PHASE_CONF_CD': 'S190009',
      'B_RING_2_PHASE_CONF_CD': 'L075193',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S013192',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L255023',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'P271',
      'INT_NO': '761',
      'A_RING_4_PHASE_CONF_CD': 'L270360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S180360',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S359180',
      'INT_NM': '정금마을',
      'A_RING_3_PHASE_CONF_CD': 'P271',
      'B_RING_2_PHASE_CONF_CD': None,
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'P271',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P270',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'P088',
      'INT_NO': '761',
      'A_RING_4_PHASE_CONF_CD': 'L270360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S180360',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S359180',
      'INT_NM': '정금마을',
      'A_RING_3_PHASE_CONF_CD': 'P089',
      'B_RING_2_PHASE_CONF_CD': None,
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'P088',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P090',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L095184',
      'INT_NO': '767',
      'A_RING_4_PHASE_CONF_CD': 'L275005',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S184004',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S004185',
      'INT_NM': '명덕초교입구',
      'A_RING_3_PHASE_CONF_CD': 'P277',
      'B_RING_2_PHASE_CONF_CD': 'L184274',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'P278',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L004095',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S358178',
      'INT_NO': '772',
      'A_RING_4_PHASE_CONF_CD': 'L358087',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S088268',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S268088',
      'INT_NM': '평화약국',
      'A_RING_3_PHASE_CONF_CD': 'S178358',
      'B_RING_2_PHASE_CONF_CD': 'S268088',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L178267',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S088268',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S357176',
      'INT_NO': '772',
      'A_RING_4_PHASE_CONF_CD': 'L357087',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S087267',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S267087',
      'INT_NM': '평화약국',
      'A_RING_3_PHASE_CONF_CD': 'S176357',
      'B_RING_2_PHASE_CONF_CD': 'S267087',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'L176267',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S087267',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '774',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S108287',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S288107',
      'INT_NM': '방배래미안타워',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P017',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L287021',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '775',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S101281',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S281101',
      'INT_NM': '신성택시',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P011',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L281011',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '776',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S060239',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S240059',
      'INT_NM': '경남아파트앞',
      'A_RING_3_PHASE_CONF_CD': 'L329060',
      'B_RING_2_PHASE_CONF_CD': 'P330',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'P331',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L239330',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '776',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S060239',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S240059',
      'INT_NM': '경남아파트앞',
      'A_RING_3_PHASE_CONF_CD': 'L329060',
      'B_RING_2_PHASE_CONF_CD': 'P330',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'P330',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L239330',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S277096',
      'INT_NO': '877',
      'A_RING_4_PHASE_CONF_CD': 'L278006',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S188006',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S007186',
      'INT_NM': '장평교',
      'A_RING_3_PHASE_CONF_CD': 'S098276',
      'B_RING_2_PHASE_CONF_CD': 'L187276',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S277096',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L008096',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L087176',
      'INT_NO': '878',
      'A_RING_4_PHASE_CONF_CD': 'P268',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S177355',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S357175',
      'INT_NM': '면목4치안센터',
      'A_RING_3_PHASE_CONF_CD': 'L357087',
      'B_RING_2_PHASE_CONF_CD': 'S357175',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S357175',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S177355',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L090180',
      'INT_NO': '880',
      'A_RING_4_PHASE_CONF_CD': 'S090270',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': None,
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S360180',
      'INT_NM': '중랑전화국',
      'A_RING_3_PHASE_CONF_CD': 'S090270',
      'B_RING_2_PHASE_CONF_CD': 'L180270',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S270090',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L360090',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L093183',
      'INT_NO': '881',
      'A_RING_4_PHASE_CONF_CD': 'S093273',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S183003',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S003183',
      'INT_NM': '면목2동',
      'A_RING_3_PHASE_CONF_CD': 'L273003',
      'B_RING_2_PHASE_CONF_CD': 'L183273',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S273093',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L003093',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L082172',
      'INT_NO': '883',
      'A_RING_4_PHASE_CONF_CD': 'L262352',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S172353',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S352173',
      'INT_NM': '묵2동',
      'A_RING_3_PHASE_CONF_CD': 'P260',
      'B_RING_2_PHASE_CONF_CD': 'S352173',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L172262',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S172353',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'P269',
      'INT_NO': '896',
      'A_RING_4_PHASE_CONF_CD': 'L270360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S180360',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S360180',
      'INT_NM': '수락산역',
      'A_RING_3_PHASE_CONF_CD': 'P268',
      'B_RING_2_PHASE_CONF_CD': 'S360180',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L180270',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S180360',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L074164',
      'INT_NO': '899',
      'A_RING_4_PHASE_CONF_CD': 'S075254',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S165344',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S345164',
      'INT_NM': '상계백병원',
      'A_RING_3_PHASE_CONF_CD': 'L254344',
      'B_RING_2_PHASE_CONF_CD': 'L164254',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S255074',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L344074',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '922',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S204025',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S024205',
      'INT_NM': '동곡삼거리',
      'A_RING_3_PHASE_CONF_CD': 'P295',
      'B_RING_2_PHASE_CONF_CD': 'P296',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L115205',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L025115',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '927',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S111291',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S291111',
      'INT_NM': '올림픽대교남단',
      'A_RING_3_PHASE_CONF_CD': 'S200021',
      'B_RING_2_PHASE_CONF_CD': 'S021201',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L201291',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L021111',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S326146',
      'INT_NO': '928',
      'A_RING_4_PHASE_CONF_CD': 'L325056',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S056236',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': 'P326',
      'B_RING_5_PHASE_CONF_CD': 'L055146',
      'B_RING_1_PHASE_CONF_CD': 'S235055',
      'INT_NM': '신가초교앞',
      'A_RING_3_PHASE_CONF_CD': 'S146326',
      'B_RING_2_PHASE_CONF_CD': 'P326',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L145236',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L235326',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '929',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'L245335',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S244066',
      'INT_NM': '농협신설동지점',
      'A_RING_3_PHASE_CONF_CD': 'P335',
      'B_RING_2_PHASE_CONF_CD': 'S244066',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'P335',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S064246',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L210300',
      'INT_NO': '936',
      'A_RING_4_PHASE_CONF_CD': 'L030120',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S120300',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S300120',
      'INT_NM': '광장사거리',
      'A_RING_3_PHASE_CONF_CD': 'S210030',
      'B_RING_2_PHASE_CONF_CD': 'L120210',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S030210',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L300030',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S298117',
      'INT_NO': '940',
      'A_RING_4_PHASE_CONF_CD': 'L298026',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S028206',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S208026',
      'INT_NM': '방이역',
      'A_RING_3_PHASE_CONF_CD': 'S118296',
      'B_RING_2_PHASE_CONF_CD': 'L028116',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L118206',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L208297',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '999',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S077255',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S257075',
      'INT_NM': '호림박물관입구',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'L077134',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L257310',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1015',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S198015',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S018196',
      'INT_NM': '대한볼링협회',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P288',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P287',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L195286',
      'INT_NO': '1018',
      'A_RING_4_PHASE_CONF_CD': 'S196015',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S106285',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S286106',
      'INT_NM': '서하남IC입구',
      'A_RING_3_PHASE_CONF_CD': 'L015105',
      'B_RING_2_PHASE_CONF_CD': 'L105195',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S015195',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L285015',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1202',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S106285',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S286105',
      'INT_NM': '병천순대',
      'A_RING_3_PHASE_CONF_CD': 'P015',
      'B_RING_2_PHASE_CONF_CD': 'S286105',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'P016',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S106285',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S270090',
      'INT_NO': '1400',
      'A_RING_4_PHASE_CONF_CD': 'L270360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': None,
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S360180',
      'INT_NM': '중화역',
      'A_RING_3_PHASE_CONF_CD': 'S090270',
      'B_RING_2_PHASE_CONF_CD': 'L180270',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L090180',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L360090',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S357175',
      'INT_NO': '1816',
      'A_RING_4_PHASE_CONF_CD': 'L356087',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S087265',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S267085',
      'INT_NM': '종로4가',
      'A_RING_3_PHASE_CONF_CD': 'S177355',
      'B_RING_2_PHASE_CONF_CD': 'S267085',
      'MAP_NO': '2',
      'B_RING_3_PHASE_CONF_CD': 'S357175',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S087265',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L180270',
      'INT_NO': '1819',
      'A_RING_4_PHASE_CONF_CD': 'L360090',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S090270',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S270090',
      'INT_NM': '종로1가',
      'A_RING_3_PHASE_CONF_CD': 'S180360',
      'B_RING_2_PHASE_CONF_CD': 'L090180',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'S360179',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S090270',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L178268',
      'INT_NO': '1820',
      'A_RING_4_PHASE_CONF_CD': 'S178359',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S088270',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': 'L358088',
      'B_RING_5_PHASE_CONF_CD': 'L178268',
      'B_RING_1_PHASE_CONF_CD': 'S268090',
      'INT_NM': '종로2가',
      'A_RING_3_PHASE_CONF_CD': 'S178359',
      'B_RING_2_PHASE_CONF_CD': 'L088178',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S358180',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S088270',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1821',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S090270',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S270090',
      'INT_NM': '세종대로사거리',
      'A_RING_3_PHASE_CONF_CD': 'S180360',
      'B_RING_2_PHASE_CONF_CD': 'S360180',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L180270',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S180360',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1821',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S090270',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S270090',
      'INT_NM': '세종대로사거리',
      'A_RING_3_PHASE_CONF_CD': 'P180',
      'B_RING_2_PHASE_CONF_CD': 'P360',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'L180270',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P180',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1863',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S083265',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S263085',
      'INT_NM': '종로3가',
      'A_RING_3_PHASE_CONF_CD': 'L353085',
      'B_RING_2_PHASE_CONF_CD': 'P353',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L174265',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S173355',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L177267',
      'INT_NO': '1863',
      'A_RING_4_PHASE_CONF_CD': 'L356087',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S087267',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S267087',
      'INT_NM': '종로3가',
      'A_RING_3_PHASE_CONF_CD': 'S177356',
      'B_RING_2_PHASE_CONF_CD': 'S267087',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'P357',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L267356',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S352174',
      'INT_NO': '1865',
      'A_RING_4_PHASE_CONF_CD': 'S172354',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S082263',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S262083',
      'INT_NM': '동묘앞역',
      'A_RING_3_PHASE_CONF_CD': 'L262352',
      'B_RING_2_PHASE_CONF_CD': 'S262083',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L082172',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L262352',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1869',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S197017',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S017197',
      'INT_NM': '둔촌아파트입구삼거리',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'L107199',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P288',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1869',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S197017',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S017197',
      'INT_NM': '둔촌아파트입구삼거리',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'L107199',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P286',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1871',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S098276',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S278096',
      'INT_NM': 'LG전자',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P007',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P007',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1873',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S114295',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'L115205',
      'INT_NM': '낙성대입구',
      'A_RING_3_PHASE_CONF_CD': 'S204025',
      'B_RING_2_PHASE_CONF_CD': 'S294115',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L205295',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S114295',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L056147',
      'INT_NO': '1882',
      'A_RING_4_PHASE_CONF_CD': 'L056147',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S146326',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S326146',
      'INT_NM': '강서운전면허시험장입구',
      'A_RING_3_PHASE_CONF_CD': 'L326057',
      'B_RING_2_PHASE_CONF_CD': 'S326146',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S326146',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S146326',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1911',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S203024',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S023204',
      'INT_NM': '동일로주유소',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P293',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P292',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S019198',
      'INT_NO': '1915',
      'A_RING_4_PHASE_CONF_CD': 'L017076',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S058237',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S238057',
      'INT_NM': '둔촌고교입구',
      'A_RING_3_PHASE_CONF_CD': 'S198017',
      'B_RING_2_PHASE_CONF_CD': 'L057199',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L200261',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L237016',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S290112',
      'INT_NO': '1929',
      'A_RING_4_PHASE_CONF_CD': 'L291022',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S201021',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': 'L291022',
      'B_RING_5_PHASE_CONF_CD': 'P021',
      'B_RING_1_PHASE_CONF_CD': 'S021201',
      'INT_NM': '서울역R',
      'A_RING_3_PHASE_CONF_CD': 'S111292',
      'B_RING_2_PHASE_CONF_CD': 'L200292',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S290112',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S201021',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1943',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S113294',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S293114',
      'INT_NM': '시민교회',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P023',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P023',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1946',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S179360',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S179360',
      'INT_NM': '청계광장',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P359',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P359',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'S024204',
      'INT_NO': '1954',
      'A_RING_4_PHASE_CONF_CD': 'L024115',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S114294',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S294114',
      'INT_NM': '군자역',
      'A_RING_3_PHASE_CONF_CD': 'S204024',
      'B_RING_2_PHASE_CONF_CD': 'L114205',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L204295',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L294025',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '1989',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S118297',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S298116',
      'INT_NM': '성내천공원앞',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P028',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L028117',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '2031',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S123300',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S303121',
      'INT_NM': '청계한신휴플러스',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P033',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L032300',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '2152',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S181360',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S001',
      'INT_NM': '수락초교삼거리',
      'A_RING_3_PHASE_CONF_CD': 'L271001',
      'B_RING_2_PHASE_CONF_CD': 'L181271',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L271001',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S181360',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '2153',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S065246',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S245066',
      'INT_NM': '늘푸른교회',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P335',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P335',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '2155',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S112294',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S292114',
      'INT_NM': '철교밑',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'L112236',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L112236',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'L199287',
      'INT_NO': '2157',
      'A_RING_4_PHASE_CONF_CD': 'L019107',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S108288',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'L109197',
      'INT_NM': '천호사거리',
      'A_RING_3_PHASE_CONF_CD': 'S198018',
      'B_RING_2_PHASE_CONF_CD': 'S288108',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S018198',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L290017',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '2900',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S201021',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S021201',
      'INT_NM': '숭례문',
      'A_RING_3_PHASE_CONF_CD': 'S111291',
      'B_RING_2_PHASE_CONF_CD': 'S021201',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L110202',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S180001',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '2900',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S201021',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S021201',
      'INT_NM': '숭례문',
      'A_RING_3_PHASE_CONF_CD': 'S111291',
      'B_RING_2_PHASE_CONF_CD': 'S021201',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'L110202',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S180001',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '3100',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S098277',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S278097',
      'INT_NM': '봉천역',
      'A_RING_3_PHASE_CONF_CD': None,
      'B_RING_2_PHASE_CONF_CD': 'P007',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': None,
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'P008',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '3200',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S198016',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S018196',
      'INT_NM': '둔촌동역',
      'A_RING_3_PHASE_CONF_CD': 'S108286',
      'B_RING_2_PHASE_CONF_CD': 'S288106',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'L108196',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L288016',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': None,
      'INT_NO': '3200',
      'A_RING_4_PHASE_CONF_CD': None,
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S198016',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S018196',
      'INT_NM': '둔촌동역',
      'A_RING_3_PHASE_CONF_CD': 'S108286',
      'B_RING_2_PHASE_CONF_CD': 'S288106',
      'MAP_NO': '1',
      'B_RING_3_PHASE_CONF_CD': 'L108196',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'L288016',
      'B_RING_6_PHASE_CONF_CD': None},
     {'REGION_CD': 'L01',
      'A_RING_6_PHASE_CONF_CD': None,
      'INT_MAINPHASE': '1',
      'A_RING_7_PHASE_CONF_CD': None,
      'B_RING_4_PHASE_CONF_CD': 'P360',
      'INT_NO': '6800',
      'A_RING_4_PHASE_CONF_CD': 'P360',
      'A_RING_8_PHASE_CONF_CD': None,
      'A_RING_1_PHASE_CONF_CD': 'S090270',
      'B_RING_8_PHASE_CONF_CD': None,
      'A_RING_5_PHASE_CONF_CD': None,
      'B_RING_5_PHASE_CONF_CD': None,
      'B_RING_1_PHASE_CONF_CD': 'S270090',
      'INT_NM': '연1)이대역',
      'A_RING_3_PHASE_CONF_CD': 'S090270',
      'B_RING_2_PHASE_CONF_CD': 'S270090',
      'MAP_NO': '0',
      'B_RING_3_PHASE_CONF_CD': 'S270090',
      'B_RING_7_PHASE_CONF_CD': None,
      'A_RING_2_PHASE_CONF_CD': 'S090270',
      'B_RING_6_PHASE_CONF_CD': None}]




```python
data = pd.json_normalize(json)
#data.to_csv(path+'int_xy.csv', encoding="EUC-KR", index=False)
data
```




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
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>resultMsg</th>
      <th>totPage</th>
      <th>totCnt</th>
      <th>pageNo</th>
      <th>resultCode</th>
      <th>numOfRows</th>
      <th>REGION_CD</th>
      <th>A_RING_6_PHASE_CONF_CD</th>
      <th>INT_MAINPHASE</th>
      <th>A_RING_7_PHASE_CONF_CD</th>
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
      <td>NORMAL_SERVICE</td>
      <td>47</td>
      <td>470</td>
      <td>1</td>
      <td>0</td>
      <td>100</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S353173</td>
      <td>영동대교남단</td>
      <td>S173354</td>
      <td>S353173</td>
      <td>0</td>
      <td>L172262</td>
      <td>NaN</td>
      <td>S173354</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S262080</td>
      <td>원앙예식장</td>
      <td>None</td>
      <td>P352</td>
      <td>0</td>
      <td>None</td>
      <td>NaN</td>
      <td>P352</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S259080</td>
      <td>청담사거리</td>
      <td>S160340</td>
      <td>L080171</td>
      <td>0</td>
      <td>S340161</td>
      <td>NaN</td>
      <td>L260351</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S260081</td>
      <td>청담사거리</td>
      <td>S170351</td>
      <td>L080172</td>
      <td>1</td>
      <td>S350171</td>
      <td>NaN</td>
      <td>L260352</td>
      <td>NaN</td>
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
      <th>96</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S021201</td>
      <td>숭례문</td>
      <td>S111291</td>
      <td>S021201</td>
      <td>1</td>
      <td>L110202</td>
      <td>NaN</td>
      <td>S180001</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>97</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S278097</td>
      <td>봉천역</td>
      <td>None</td>
      <td>P007</td>
      <td>0</td>
      <td>None</td>
      <td>NaN</td>
      <td>P008</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>98</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S018196</td>
      <td>둔촌동역</td>
      <td>S108286</td>
      <td>S288106</td>
      <td>0</td>
      <td>L108196</td>
      <td>NaN</td>
      <td>L288016</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>99</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S018196</td>
      <td>둔촌동역</td>
      <td>S108286</td>
      <td>S288106</td>
      <td>1</td>
      <td>L108196</td>
      <td>NaN</td>
      <td>L288016</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>100</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>L01</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>None</td>
      <td>S270090</td>
      <td>연1)이대역</td>
      <td>S090270</td>
      <td>S270090</td>
      <td>0</td>
      <td>S270090</td>
      <td>NaN</td>
      <td>S090270</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>101 rows × 27 columns</p>
</div>



# 지점 위치


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




    <?xml version="1.0" encoding="UTF-8" standalone="yes"?><spotinfo><list_total_count>169</list_total_count><result><code>INFO-000</code><message>정상 처리되었습니다</message></result><row><spot_num>A-01</spot_num><spot_nm>성산로(금화터널)</spot_nm><grs80tm_x>195489</grs80tm_x><grs80tm_y>452136</grs80tm_y></row><row><spot_num>A-02</spot_num><spot_nm>사직로(사직터널)</spot_nm><grs80tm_x>196756.776106</grs80tm_x><grs80tm_y>452546.638644</grs80tm_y></row><row><spot_num>A-03</spot_num><spot_nm>자하문로(자하문터널)</spot_nm><grs80tm_x>197216.855046</grs80tm_x><grs80tm_y>454350.990432</grs80tm_y></row><row><spot_num>A-04</spot_num><spot_nm>대사관로(삼청터널)</spot_nm><grs80tm_x>198648.893154</grs80tm_x><grs80tm_y>455200.108465</grs80tm_y></row><row><spot_num>A-05</spot_num><spot_nm>율곡로(안국역)</spot_nm><grs80tm_x>198645.671347</grs80tm_x><grs80tm_y>452937.216603</grs80tm_y></row><row><spot_num>A-06</spot_num><spot_nm>창경궁로(서울여자대학교)</spot_nm><grs80tm_x>199825.89671</grs80tm_x><grs80tm_y>453668.322568</grs80tm_y></row><row><spot_num>A-07</spot_num><spot_nm>대학로(한국방송통신대학교)</spot_nm><grs80tm_x>200185.014636</grs80tm_x><grs80tm_y>453144.745427</grs80tm_y></row><row><spot_num>A-08</spot_num><spot_nm>종로(동묘앞역)</spot_nm><grs80tm_x>201515.783975</grs80tm_x><grs80tm_y>452624.900832</grs80tm_y></row><row><spot_num>A-09</spot_num><spot_nm>퇴계로(신당역)</spot_nm><grs80tm_x>201850.055284</grs80tm_x><grs80tm_y>451769.96985</grs80tm_y></row><row><spot_num>A-10</spot_num><spot_nm>동호로(장충체육관)</spot_nm><grs80tm_x>200633.487975</grs80tm_x><grs80tm_y>451010.051429</grs80tm_y></row><row><spot_num>A-11</spot_num><spot_nm>장충단로(장충단공원)</spot_nm><grs80tm_x>200410</grs80tm_x><grs80tm_y>450804</grs80tm_y></row><row><spot_num>A-12</spot_num><spot_nm>퇴계로(회현역)</spot_nm><grs80tm_x>197939.908023</grs80tm_x><grs80tm_y>450888.928346</grs80tm_y></row><row><spot_num>A-13</spot_num><spot_nm>세종대로(서울역)</spot_nm><grs80tm_x>197727.33407</grs80tm_x><grs80tm_y>451006.009692</grs80tm_y></row><row><spot_num>A-14</spot_num><spot_nm>새문안로(서울역사박물관)</spot_nm><grs80tm_x>197432.421007</grs80tm_x><grs80tm_y>452243.292862</grs80tm_y></row><row><spot_num>A-15</spot_num><spot_nm>종로(종로3가역)</spot_nm><grs80tm_x>199240.067506</grs80tm_x><grs80tm_y>452288.413612</grs80tm_y></row><row><spot_num>A-16</spot_num><spot_nm>서소문로(시청역)</spot_nm><grs80tm_x>197596.4694</grs80tm_x><grs80tm_y>451463.528762</grs80tm_y></row><row><spot_num>A-17</spot_num><spot_nm>세종대로(시청역2)</spot_nm><grs80tm_x>197983.404032</grs80tm_x><grs80tm_y>452006.746671</grs80tm_y></row><row><spot_num>A-18</spot_num><spot_nm>을지로(을지로3가역)</spot_nm><grs80tm_x>199025.306571</grs80tm_x><grs80tm_y>451837.790166</grs80tm_y></row><row><spot_num>A-19</spot_num><spot_nm>칠패로(숭례문)</spot_nm><grs80tm_x>197579.482653</grs80tm_x><grs80tm_y>451141.212855</grs80tm_y></row><row><spot_num>A-20</spot_num><spot_nm>남산1호터널</spot_nm><grs80tm_x>200375.908853</grs80tm_x><grs80tm_y>448730.787276</grs80tm_y></row><row><spot_num>A-21</spot_num><spot_nm>남산2호터널</spot_nm><grs80tm_x>199098.824842</grs80tm_x><grs80tm_y>449434.634668</grs80tm_y></row><row><spot_num>A-22</spot_num><spot_nm>남산3호터널</spot_nm><grs80tm_x>198985.638133</grs80tm_x><grs80tm_y>449446.700998</grs80tm_y></row><row><spot_num>A-23</spot_num><spot_nm>소월로(회현역)</spot_nm><grs80tm_x>197913</grs80tm_x><grs80tm_y>450842</grs80tm_y></row><row><spot_num>A-24</spot_num><spot_nm>소파로(숭의여자대학교)</spot_nm><grs80tm_x>198564</grs80tm_x><grs80tm_y>450618</grs80tm_y></row><row><spot_num>B-01</spot_num><spot_nm>도봉로(도봉산역)</spot_nm><grs80tm_x>203959</grs80tm_x><grs80tm_y>465826</grs80tm_y></row><row><spot_num>B-02</spot_num><spot_nm>동일로(의정부IC)</spot_nm><grs80tm_x>204889.526551</grs80tm_x><grs80tm_y>465841.155869</grs80tm_y></row><row><spot_num>B-03</spot_num><spot_nm>아차산로(워커힐)</spot_nm><grs80tm_x>209697.184721</grs80tm_x><grs80tm_y>450264.164859</grs80tm_y></row><row><spot_num>B-04</spot_num><spot_nm>망우로(망우리공원)</spot_nm><grs80tm_x>210241.085553</grs80tm_x><grs80tm_y>455754.962803</grs80tm_y></row><row><spot_num>B-05</spot_num><spot_nm>경춘북로(중랑경찰서)</spot_nm><grs80tm_x>209331.347572</grs80tm_x><grs80tm_y>457857.713789</grs80tm_y></row><row><spot_num>B-06</spot_num><spot_nm>화랑로(조선왕릉)</spot_nm><grs80tm_x>208764</grs80tm_x><grs80tm_y>459019</grs80tm_y></row><row><spot_num>B-07</spot_num><spot_nm>북부간선도로(신내IC)</spot_nm><grs80tm_x>209618</grs80tm_x><grs80tm_y>457126</grs80tm_y></row><row><spot_num>B-08</spot_num><spot_nm>서하남로(서하남IC)</spot_nm><grs80tm_x>213005.890193</grs80tm_x><grs80tm_y>446398.999033</grs80tm_y></row><row><spot_num>B-09</spot_num><spot_nm>천호대로(상일IC)</spot_nm><grs80tm_x>215469.587229</grs80tm_x><grs80tm_y>449773.571726</grs80tm_y></row><row><spot_num>B-10</spot_num><spot_nm>올림픽대로(강일IC)</spot_nm><grs80tm_x>213894</grs80tm_x><grs80tm_y>452265</grs80tm_y></row><row><spot_num>B-11</spot_num><spot_nm>경부고속도로(양재IC)</spot_nm><grs80tm_x>203414</grs80tm_x><grs80tm_y>440621</grs80tm_y></row><row><spot_num>B-12</spot_num><spot_nm>송파대로(복정역)</spot_nm><grs80tm_x>211162.986746</grs80tm_x><grs80tm_y>441076.417571</grs80tm_y></row><row><spot_num>B-13</spot_num><spot_nm>밤고개로(세곡동사거리)</spot_nm><grs80tm_x>209531</grs80tm_x><grs80tm_y>440360</grs80tm_y></row><row><spot_num>B-14</spot_num><spot_nm>분당-수서로(성남시계)</spot_nm><grs80tm_x>210863</grs80tm_x><grs80tm_y>441329</grs80tm_y></row><row><spot_num>B-15</spot_num><spot_nm>과천대로(남태령역)</spot_nm><grs80tm_x>198992.883246</grs80tm_x><grs80tm_y>440448.530816</grs80tm_y></row><row><spot_num>B-16</spot_num><spot_nm>양재대로(양재IC)</spot_nm><grs80tm_x>203101</grs80tm_x><grs80tm_y>440084</grs80tm_y></row><row><spot_num>B-17</spot_num><spot_nm>반포대로(우면산터널)</spot_nm><grs80tm_x>201006.172398</grs80tm_x><grs80tm_y>442700.15907</grs80tm_y></row><row><spot_num>B-18</spot_num><spot_nm>시흥대로(석수역)</spot_nm><grs80tm_x>191458.931086</grs80tm_x><grs80tm_y>437292.624732</grs80tm_y></row><row><spot_num>B-19</spot_num><spot_nm>금오로(광명시계)</spot_nm><grs80tm_x>186009.859895</grs80tm_x><grs80tm_y>442604.947339</grs80tm_y></row><row><spot_num>B-20</spot_num><spot_nm>오리로(광명시계)</spot_nm><grs80tm_x>186105</grs80tm_x><grs80tm_y>442593</grs80tm_y></row><row><spot_num>B-21</spot_num><spot_nm>개봉로(개봉교)</spot_nm><grs80tm_x>187302.087526</grs80tm_x><grs80tm_y>443031.264703</grs80tm_y></row><row><spot_num>B-22</spot_num><spot_nm>광명대교(광명시계)</spot_nm><grs80tm_x>188799.726093</grs80tm_x><grs80tm_y>442845.631707</grs80tm_y></row><row><spot_num>B-23</spot_num><spot_nm>철산교(광명시계)</spot_nm><grs80tm_x>189248.578812</grs80tm_x><grs80tm_y>441676.103787</grs80tm_y></row><row><spot_num>B-24</spot_num><spot_nm>금천교(광명시계)</spot_nm><grs80tm_x>189776.878856</grs80tm_x><grs80tm_y>440585.948941</grs80tm_y></row><row><spot_num>B-25</spot_num><spot_nm>금하로(광명시계)</spot_nm><grs80tm_x>190423.225984</grs80tm_x><grs80tm_y>439111.924802</grs80tm_y></row><row><spot_num>B-26</spot_num><spot_nm>오정로(부천시계)</spot_nm><grs80tm_x>183158.839363</grs80tm_x><grs80tm_y>449254.914663</grs80tm_y></row><row><spot_num>B-27</spot_num><spot_nm>화곡로(화곡로입구)</spot_nm><grs80tm_x>184478</grs80tm_x><grs80tm_y>448880</grs80tm_y></row><row><spot_num>B-28</spot_num><spot_nm>경인고속국도(신월IC)</spot_nm><grs80tm_x>185181</grs80tm_x><grs80tm_y>447307</grs80tm_y></row><row><spot_num>B-29</spot_num><spot_nm>경인로(유한공고)</spot_nm><grs80tm_x>184313.594374</grs80tm_x><grs80tm_y>443266.257648</grs80tm_y></row><row><spot_num>B-30</spot_num><spot_nm>신정로(작동터널)</spot_nm><grs80tm_x>184448</grs80tm_x><grs80tm_y>445203</grs80tm_y></row><row><spot_num>B-31</spot_num><spot_nm>김포대로(개화교)</spot_nm><grs80tm_x>181967.262021</grs80tm_x><grs80tm_y>454014.878725</grs80tm_y></row><row><spot_num>B-32</spot_num><spot_nm>올림픽대로(개화IC)</spot_nm><grs80tm_x>183484</grs80tm_x><grs80tm_y>454270</grs80tm_y></row><row><spot_num>B-33</spot_num><spot_nm>통일로(고양시계)</spot_nm><grs80tm_x>191766.010683</grs80tm_x><grs80tm_y>460807.999962</grs80tm_y></row><row><spot_num>B-34</spot_num><spot_nm>서오릉로(고양시계)</spot_nm><grs80tm_x>191705.649189</grs80tm_x><grs80tm_y>457593.577066</grs80tm_y></row><row><spot_num>B-35</spot_num><spot_nm>수색로(고양시계)</spot_nm><grs80tm_x>189971.537695</grs80tm_x><grs80tm_y>454250.346428</grs80tm_y></row><row><spot_num>B-36</spot_num><spot_nm>강변북로(난지한강공원)</spot_nm><grs80tm_x>188739</grs80tm_x><grs80tm_y>452378</grs80tm_y></row><row><spot_num>B-37</spot_num><spot_nm>강변북로(구리시계)</spot_nm><grs80tm_x>210360</grs80tm_x><grs80tm_y>451547</grs80tm_y></row><row><spot_num>C-01</spot_num><spot_nm>행주대교</spot_nm><grs80tm_x>183194</grs80tm_x><grs80tm_y>455379</grs80tm_y></row><row><spot_num>C-02</spot_num><spot_nm>방화대교</spot_nm><grs80tm_x>184719</grs80tm_x><grs80tm_y>454345</grs80tm_y></row><row><spot_num>C-03</spot_num><spot_nm>가양대교</spot_nm><grs80tm_x>187636</grs80tm_x><grs80tm_y>452084</grs80tm_y></row><row><spot_num>C-04</spot_num><spot_nm>성산대교</spot_nm><grs80tm_x>190385</grs80tm_x><grs80tm_y>450291</grs80tm_y></row><row><spot_num>C-05</spot_num><spot_nm>양화대교</spot_nm><grs80tm_x>191513</grs80tm_x><grs80tm_y>449316</grs80tm_y></row><row><spot_num>C-06</spot_num><spot_nm>서강대교</spot_nm><grs80tm_x>193396</grs80tm_x><grs80tm_y>448653</grs80tm_y></row><row><spot_num>C-07</spot_num><spot_nm>마포대교</spot_nm><grs80tm_x>194380</grs80tm_x><grs80tm_y>448236</grs80tm_y></row><row><spot_num>C-08</spot_num><spot_nm>원효대교</spot_nm><grs80tm_x>195164</grs80tm_x><grs80tm_y>447478</grs80tm_y></row><row><spot_num>C-09</spot_num><spot_nm>한강대교</spot_nm><grs80tm_x>196388</grs80tm_x><grs80tm_y>446508</grs80tm_y></row><row><spot_num>C-10</spot_num><spot_nm>동작대교</spot_nm><grs80tm_x>198378</grs80tm_x><grs80tm_y>445569</grs80tm_y></row><row><spot_num>C-11</spot_num><spot_nm>반포대교</spot_nm><grs80tm_x>199675</grs80tm_x><grs80tm_y>446138</grs80tm_y></row><row><spot_num>C-12</spot_num><spot_nm>잠수교</spot_nm><grs80tm_x>200000.318299</grs80tm_x><grs80tm_y>445351.121605</grs80tm_y></row><row><spot_num>C-13</spot_num><spot_nm>한남대교</spot_nm><grs80tm_x>201149</grs80tm_x><grs80tm_y>447494</grs80tm_y></row><row><spot_num>C-14</spot_num><spot_nm>동호대교</spot_nm><grs80tm_x>201721</grs80tm_x><grs80tm_y>448748</grs80tm_y></row><row><spot_num>C-15</spot_num><spot_nm>성수대교</spot_nm><grs80tm_x>203083</grs80tm_x><grs80tm_y>448599</grs80tm_y></row><row><spot_num>C-16</spot_num><spot_nm>영동대교</spot_nm><grs80tm_x>205070</grs80tm_x><grs80tm_y>447872</grs80tm_y></row><row><spot_num>C-17</spot_num><spot_nm>청담대교</spot_nm><grs80tm_x>205655</grs80tm_x><grs80tm_y>447331</grs80tm_y></row><row><spot_num>C-18</spot_num><spot_nm>잠실대교</spot_nm><grs80tm_x>208104</grs80tm_x><grs80tm_y>447180</grs80tm_y></row><row><spot_num>C-19</spot_num><spot_nm>올림픽대교</spot_nm><grs80tm_x>209189</grs80tm_x><grs80tm_y>448263</grs80tm_y></row><row><spot_num>C-20</spot_num><spot_nm>천호대교</spot_nm><grs80tm_x>209958</grs80tm_x><grs80tm_y>449248</grs80tm_y></row><row><spot_num>C-21</spot_num><spot_nm>광진교</spot_nm><grs80tm_x>210180</grs80tm_x><grs80tm_y>449414</grs80tm_y></row><row><spot_num>C-22</spot_num><spot_nm>강동대교</spot_nm><grs80tm_x>214266</grs80tm_x><grs80tm_y>453125</grs80tm_y></row><row><spot_num>D-01</spot_num><spot_nm>진흥로(구기터널)</spot_nm><grs80tm_x>195995.937882</grs80tm_x><grs80tm_y>456587.128855</grs80tm_y></row><row><spot_num>D-02</spot_num><spot_nm>평창문화로(북악터널)</spot_nm><grs80tm_x>198182.233352</grs80tm_x><grs80tm_y>456877.108256</grs80tm_y></row><row><spot_num>D-03</spot_num><spot_nm>동호로(금호터널)</spot_nm><grs80tm_x>201186.751155</grs80tm_x><grs80tm_y>450249.111193</grs80tm_y></row><row><spot_num>D-04</spot_num><spot_nm>서빙고로(한남역)</spot_nm><grs80tm_x>200406</grs80tm_x><grs80tm_y>447518</grs80tm_y></row><row><spot_num>D-05</spot_num><spot_nm>천호대로(군자교)</spot_nm><grs80tm_x>206060.199166</grs80tm_x><grs80tm_y>451252.982968</grs80tm_y></row><row><spot_num>D-06</spot_num><spot_nm>뚝섬로(용비교)</spot_nm><grs80tm_x>201813</grs80tm_x><grs80tm_y>449171</grs80tm_y></row><row><spot_num>D-07</spot_num><spot_nm>동일로(군자교)</spot_nm><grs80tm_x>206209</grs80tm_x><grs80tm_y>450406</grs80tm_y></row><row><spot_num>D-08</spot_num><spot_nm>화랑로(상월곡역)</spot_nm><grs80tm_x>203909.428342</grs80tm_x><grs80tm_y>456019.24139</grs80tm_y></row><row><spot_num>D-09</spot_num><spot_nm>동소문로(길음교사거리)</spot_nm><grs80tm_x>201917.236433</grs80tm_x><grs80tm_y>455587.648946</grs80tm_y></row><row><spot_num>D-10</spot_num><spot_nm>화랑로(화랑대역)</spot_nm><grs80tm_x>207303</grs80tm_x><grs80tm_y>457807</grs80tm_y></row><row><spot_num>D-11</spot_num><spot_nm>도봉로(쌍문역)</spot_nm><grs80tm_x>202948.26258</grs80tm_x><grs80tm_y>460673.661905</grs80tm_y></row><row><spot_num>D-12</spot_num><spot_nm>동부간선도로(월계1교)</spot_nm><grs80tm_x>205657</grs80tm_x><grs80tm_y>459221</grs80tm_y></row><row><spot_num>D-13</spot_num><spot_nm>동일로(노원역)</spot_nm><grs80tm_x>205351.935482</grs80tm_x><grs80tm_y>461419.376043</grs80tm_y></row><row><spot_num>D-14</spot_num><spot_nm>증산로(디지털미디어시티역)</spot_nm><grs80tm_x>191636.082633</grs80tm_x><grs80tm_y>453367.561257</grs80tm_y></row><row><spot_num>D-15</spot_num><spot_nm>통일로(산골고개정류장)</spot_nm><grs80tm_x>194715.094302</grs80tm_x><grs80tm_y>455036.774946</grs80tm_y></row><row><spot_num>D-16</spot_num><spot_nm>성산로(연희IC)</spot_nm><grs80tm_x>193927.891848</grs80tm_x><grs80tm_y>451496.144797</grs80tm_y></row><row><spot_num>D-17</spot_num><spot_nm>연희로(연희IC)</spot_nm><grs80tm_x>193843</grs80tm_x><grs80tm_y>451835</grs80tm_y></row><row><spot_num>D-18</spot_num><spot_nm>남부순환로(화곡로입구 교차로)</spot_nm><grs80tm_x>184602.679141</grs80tm_x><grs80tm_y>448970.936037</grs80tm_y></row><row><spot_num>D-19</spot_num><spot_nm>남부순환로(신월IC)</spot_nm><grs80tm_x>185530.224904</grs80tm_x><grs80tm_y>447181.128972</grs80tm_y></row><row><spot_num>D-20</spot_num><spot_nm>강서로(화곡터널)</spot_nm><grs80tm_x>186308.326778</grs80tm_x><grs80tm_y>448340.978859</grs80tm_y></row><row><spot_num>D-21</spot_num><spot_nm>공항대로(발산역)</spot_nm><grs80tm_x>184968.601439</grs80tm_x><grs80tm_y>451095.373329</grs80tm_y></row><row><spot_num>D-22</spot_num><spot_nm>경인로(오류IC)</spot_nm><grs80tm_x>186824.044955</grs80tm_x><grs80tm_y>444345.153361</grs80tm_y></row><row><spot_num>D-23</spot_num><spot_nm>경인로(거리공원입구교차로)</spot_nm><grs80tm_x>189772.781932</grs80tm_x><grs80tm_y>445222.568696</grs80tm_y></row><row><spot_num>D-24</spot_num><spot_nm>시흥대로(시흥IC)</spot_nm><grs80tm_x>191019.546672</grs80tm_x><grs80tm_y>441943.160289</grs80tm_y></row><row><spot_num>D-25</spot_num><spot_nm>영등포로(오목교)</spot_nm><grs80tm_x>189699.279303</grs80tm_x><grs80tm_y>447078.182443</grs80tm_y></row><row><spot_num>D-26</spot_num><spot_nm>시흥대로(구로디지털단지역)</spot_nm><grs80tm_x>191536.496961</grs80tm_x><grs80tm_y>443062.487406</grs80tm_y></row><row><spot_num>D-27</spot_num><spot_nm>제물포길(여의2교)</spot_nm><grs80tm_x>192081.671803</grs80tm_x><grs80tm_y>447371.898454</grs80tm_y></row><row><spot_num>D-28</spot_num><spot_nm>경인로(서울교)</spot_nm><grs80tm_x>192513.359359</grs80tm_x><grs80tm_y>446774.318707</grs80tm_y></row><row><spot_num>D-29</spot_num><spot_nm>여의대방로(여의교)</spot_nm><grs80tm_x>193620.717018</grs80tm_x><grs80tm_y>446401.753982</grs80tm_y></row><row><spot_num>D-30</spot_num><spot_nm>양녕로(상도터널)</spot_nm><grs80tm_x>195876.010078</grs80tm_x><grs80tm_y>445710.321701</grs80tm_y></row><row><spot_num>D-31</spot_num><spot_nm>동작대로(총신대입구역)</spot_nm><grs80tm_x>198482</grs80tm_x><grs80tm_y>443886</grs80tm_y></row><row><spot_num>D-32</spot_num><spot_nm>문성로(난곡터널)</spot_nm><grs80tm_x>193301.868134</grs80tm_x><grs80tm_y>442185.848969</grs80tm_y></row><row><spot_num>D-33</spot_num><spot_nm>남부순환로(낙성대역)</spot_nm><grs80tm_x>196671.683132</grs80tm_x><grs80tm_y>442047.207145</grs80tm_y></row><row><spot_num>D-34</spot_num><spot_nm>남부순환로(예술의전당)</spot_nm><grs80tm_x>200369.30044</grs80tm_x><grs80tm_y>441902.72279</grs80tm_y></row><row><spot_num>D-35</spot_num><spot_nm>강남대로(강남역-신분당)</spot_nm><grs80tm_x>202677</grs80tm_x><grs80tm_y>443638</grs80tm_y></row><row><spot_num>D-36</spot_num><spot_nm>사평대로(고속터미널역)</spot_nm><grs80tm_x>200691.392481</grs80tm_x><grs80tm_y>444896.469303</grs80tm_y></row><row><spot_num>D-37</spot_num><spot_nm>반포대로(서초역)</spot_nm><grs80tm_x>200494</grs80tm_x><grs80tm_y>444089</grs80tm_y></row><row><spot_num>D-38</spot_num><spot_nm>언주로(매봉터널)</spot_nm><grs80tm_x>204198.714027</grs80tm_x><grs80tm_y>443664.32288</grs80tm_y></row><row><spot_num>D-39</spot_num><spot_nm>남부순환로(수서IC)</spot_nm><grs80tm_x>208147.683326</grs80tm_x><grs80tm_y>444008.403275</grs80tm_y></row><row><spot_num>D-40</spot_num><spot_nm>헌릉로(세곡동사거리)</spot_nm><grs80tm_x>209322.895518</grs80tm_x><grs80tm_y>440609.887336</grs80tm_y></row><row><spot_num>D-41</spot_num><spot_nm>노들로</spot_nm><grs80tm_x>191978</grs80tm_x><grs80tm_y>447806</grs80tm_y></row><row><spot_num>D-42</spot_num><spot_nm>테헤란로(선릉역)</spot_nm><grs80tm_x>204611.157854</grs80tm_x><grs80tm_y>445158.415941</grs80tm_y></row><row><spot_num>D-43</spot_num><spot_nm>강남대로(신사역)</spot_nm><grs80tm_x>201776</grs80tm_x><grs80tm_y>446166</grs80tm_y></row><row><spot_num>D-44</spot_num><spot_nm>백제고분로(종합운동장)</spot_nm><grs80tm_x>206930</grs80tm_x><grs80tm_y>445752</grs80tm_y></row><row><spot_num>D-45</spot_num><spot_nm>송파대로(송파역)</spot_nm><grs80tm_x>210778</grs80tm_x><grs80tm_y>443092</grs80tm_y></row><row><spot_num>D-46</spot_num><spot_nm>올림픽로(송파구청~잠실역)</spot_nm><grs80tm_x>208940</grs80tm_x><grs80tm_y>446020</grs80tm_y></row><row><spot_num>D-47</spot_num><spot_nm>올림픽로(몽촌토성역~올림픽회관)</spot_nm><grs80tm_x>209998</grs80tm_x><grs80tm_y>446535</grs80tm_y></row><row><spot_num>D-48</spot_num><spot_nm>올림픽로(종합운동장)</spot_nm><grs80tm_x>207597</grs80tm_x><grs80tm_y>445803</grs80tm_y></row><row><spot_num>D-49</spot_num><spot_nm>올림픽로(잠실3사거리~잠실역)</spot_nm><grs80tm_x>208225</grs80tm_x><grs80tm_y>445834</grs80tm_y></row><row><spot_num>D-50</spot_num><spot_nm>송파대로(석촌호수(롯데월드)~석촌호수)</spot_nm><grs80tm_x>209206</grs80tm_x><grs80tm_y>445434</grs80tm_y></row><row><spot_num>D-51</spot_num><spot_nm>송파대로(잠실대교남단교차로~잠실역)</spot_nm><grs80tm_x>208895</grs80tm_x><grs80tm_y>445940</grs80tm_y></row><row><spot_num>D-52</spot_num><spot_nm>잠실로(석촌호수(롯데월드)~KT송파지사)</spot_nm><grs80tm_x>208649</grs80tm_x><grs80tm_y>445617</grs80tm_y></row><row><spot_num>D-53</spot_num><spot_nm>석촌호수로(신천역~잠실학원사거리)</spot_nm><grs80tm_x>207579</grs80tm_x><grs80tm_y>445750</grs80tm_y></row><row><spot_num>D-54</spot_num><spot_nm>석촌호수로(방이삼거리-&gt;석촌호수)</spot_nm><grs80tm_x>209229</grs80tm_x><grs80tm_y>445515</grs80tm_y></row><row><spot_num>D-55</spot_num><spot_nm>석촌호수로(석촌호(서호)-&gt;잠실학원사거리)</spot_nm><grs80tm_x>207994</grs80tm_x><grs80tm_y>445347</grs80tm_y></row><row><spot_num>D-56</spot_num><spot_nm>석촌호수로(석촌호(서호)-&gt;석촌호수)</spot_nm><grs80tm_x>209236</grs80tm_x><grs80tm_y>445497</grs80tm_y></row><row><spot_num>D-58</spot_num><spot_nm>삼전로(잠실학원사거리-&gt;잠실트리지움)</spot_nm><grs80tm_x>208248</grs80tm_x><grs80tm_y>445668</grs80tm_y></row><row><spot_num>D-59</spot_num><spot_nm>삼학사로(서울놀이마당앞-&gt;석촌호(서호))</spot_nm><grs80tm_x>208599</grs80tm_x><grs80tm_y>445225</grs80tm_y></row><row><spot_num>D-60</spot_num><spot_nm>송파대로(석촌호수-&gt;석촌역)</spot_nm><grs80tm_x>209516</grs80tm_x><grs80tm_y>445092</grs80tm_y></row><row><spot_num>D-61</spot_num><spot_nm>송파대로(잠실대교남단-&gt;잠실대교남단교차로)</spot_nm><grs80tm_x>208691</grs80tm_x><grs80tm_y>446268</grs80tm_y></row><row><spot_num>D-62</spot_num><spot_nm>신천로(올림픽회관-&gt;잠실파크리오아파트)</spot_nm><grs80tm_x>209562</grs80tm_x><grs80tm_y>446649</grs80tm_y></row><row><spot_num>D-63</spot_num><spot_nm>신천로(잠실나루역-&gt;잠실대교남단교차로)</spot_nm><grs80tm_x>208650</grs80tm_x><grs80tm_y>446277</grs80tm_y></row><row><spot_num>D-64</spot_num><spot_nm>신천로(잠실나루역-&gt;잠실대교남단교차로)</spot_nm><grs80tm_x>209087</grs80tm_x><grs80tm_y>446505</grs80tm_y></row><row><spot_num>D-65</spot_num><spot_nm>신천로(잠실나루역-&gt;잠실파크리오아파트)</spot_nm><grs80tm_x>209556</grs80tm_x><grs80tm_y>446649</grs80tm_y></row><row><spot_num>D-66</spot_num><spot_nm>신천로(잠실대교남단교차로-&gt;잠실나루역)</spot_nm><grs80tm_x>209093</grs80tm_x><grs80tm_y>446493</grs80tm_y></row><row><spot_num>D-67</spot_num><spot_nm>신천로(잠실파크리오아파트-&gt;올림픽회관)</spot_nm><grs80tm_x>210122</grs80tm_x><grs80tm_y>446756</grs80tm_y></row><row><spot_num>D-68</spot_num><spot_nm>오금로(KT송파지사-&gt;송파구청)</spot_nm><grs80tm_x>209396</grs80tm_x><grs80tm_y>446247</grs80tm_y></row><row><spot_num>D-69</spot_num><spot_nm>오금로(방이삼거리-&gt;KT송파지사)</spot_nm><grs80tm_x>209508</grs80tm_x><grs80tm_y>446042</grs80tm_y></row><row><spot_num>D-70</spot_num><spot_nm>오금로(잠실나루역-&gt;송파구청)</spot_nm><grs80tm_x>209453</grs80tm_x><grs80tm_y>446200</grs80tm_y></row><row><spot_num>D-71</spot_num><spot_nm>올림픽로(송파구청-&gt;몽촌토성역)</spot_nm><grs80tm_x>209677</grs80tm_x><grs80tm_y>446338</grs80tm_y></row><row><spot_num>D-72</spot_num><spot_nm>올림픽로(올림픽회관-&gt;몽촌토성역)</spot_nm><grs80tm_x>209964</grs80tm_x><grs80tm_y>446415</grs80tm_y></row><row><spot_num>D-73</spot_num><spot_nm>올림픽로(잠실3사거리-&gt;신천역)</spot_nm><grs80tm_x>207537</grs80tm_x><grs80tm_y>445795</grs80tm_y></row><row><spot_num>D-74</spot_num><spot_nm>올림픽로(잠실역-&gt;잠실3사거리)</spot_nm><grs80tm_x>208159</grs80tm_x><grs80tm_y>445839</grs80tm_y></row><row><spot_num>D-75</spot_num><spot_nm>올림픽로(종합운동장-&gt;신천역)</spot_nm><grs80tm_x>207593</grs80tm_x><grs80tm_y>445794</grs80tm_y></row><row><spot_num>D-76</spot_num><spot_nm>올림픽로35가길(올림픽35가사거리-&gt;홈플러스)</spot_nm><grs80tm_x>209090</grs80tm_x><grs80tm_y>446501</grs80tm_y></row><row><spot_num>D-77</spot_num><spot_nm>잠실로(잠실3사거리-&gt;잠실트리지움)</spot_nm><grs80tm_x>208246</grs80tm_x><grs80tm_y>445633</grs80tm_y></row><row><spot_num>F-01</spot_num><spot_nm>올림픽대로</spot_nm><grs80tm_x>197646</grs80tm_x><grs80tm_y>445188</grs80tm_y></row><row><spot_num>F-02</spot_num><spot_nm>강변북로</spot_nm><grs80tm_x>197729</grs80tm_x><grs80tm_y>446395</grs80tm_y></row><row><spot_num>F-03</spot_num><spot_nm>내부순환로</spot_nm><grs80tm_x>199911</grs80tm_x><grs80tm_y>456580</grs80tm_y></row><row><spot_num>F-04</spot_num><spot_nm>북부간선로</spot_nm><grs80tm_x>204649</grs80tm_x><grs80tm_y>456550</grs80tm_y></row><row><spot_num>F-05</spot_num><spot_nm>동부간선도로</spot_nm><grs80tm_x>207000</grs80tm_x><grs80tm_y>452742</grs80tm_y></row><row><spot_num>F-06</spot_num><spot_nm>경부고속도로</spot_nm><grs80tm_x>202107</grs80tm_x><grs80tm_y>443264</grs80tm_y></row><row><spot_num>F-07</spot_num><spot_nm>분당수서로</spot_nm><grs80tm_x>207716</grs80tm_x><grs80tm_y>444241</grs80tm_y></row><row><spot_num>F-08</spot_num><spot_nm>강남순환로(관악터널)</spot_nm><grs80tm_x>191832</grs80tm_x><grs80tm_y>437667</grs80tm_y></row><row><spot_num>F-09</spot_num><spot_nm>서부간선도로</spot_nm><grs80tm_x>189560</grs80tm_x><grs80tm_y>446828</grs80tm_y></row><row><spot_num>Y-53</spot_num><spot_nm>잠실로(서울놀이마당앞~잠실트리지움)</spot_nm><grs80tm_x>208208</grs80tm_x><grs80tm_y>445659</grs80tm_y></row></spotinfo>




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
</style>
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
</div>




```python
# 검지지점 선택, 좌표변환

A_09 = df_spot[df_spot['spot_num'] == "A-09"]
A_09.reset_index(drop=True, inplace=True)

xy = transform(Proj(init='epsg:5181'), Proj(init='epsg:4326'), A_09['x'][0], A_09['y'][0])
A_09['lat'] = xy[1]
A_09['lng'] = xy[0]
A_09
```

    C:\Users\jse97\anaconda3\lib\site-packages\pyproj\crs\crs.py:53: FutureWarning: '+init=<authority>:<code>' syntax is deprecated. '<authority>:<code>' is the preferred initialization method. When making the change, be mindful of axis order changes: https://pyproj4.github.io/pyproj/stable/gotchas.html#axis-order-changes-in-proj-6
      return _prepare_from_string(" ".join(pjargs))
    C:\Users\jse97\anaconda3\lib\site-packages\pyproj\crs\crs.py:294: FutureWarning: '+init=<authority>:<code>' syntax is deprecated. '<authority>:<code>' is the preferred initialization method. When making the change, be mindful of axis order changes: https://pyproj4.github.io/pyproj/stable/gotchas.html#axis-order-changes-in-proj-6
      projstring = _prepare_from_string(" ".join((projstring, projkwargs)))
    C:\Users\jse97\anaconda3\lib\site-packages\pyproj\crs\crs.py:53: FutureWarning: '+init=<authority>:<code>' syntax is deprecated. '<authority>:<code>' is the preferred initialization method. When making the change, be mindful of axis order changes: https://pyproj4.github.io/pyproj/stable/gotchas.html#axis-order-changes-in-proj-6
      return _prepare_from_string(" ".join(pjargs))
    C:\Users\jse97\anaconda3\lib\site-packages\pyproj\crs\crs.py:294: FutureWarning: '+init=<authority>:<code>' syntax is deprecated. '<authority>:<code>' is the preferred initialization method. When making the change, be mindful of axis order changes: https://pyproj4.github.io/pyproj/stable/gotchas.html#axis-order-changes-in-proj-6
      projstring = _prepare_from_string(" ".join((projstring, projkwargs)))
    <ipython-input-9-65cd4d56332c>:6: DeprecationWarning: This function is deprecated. See: https://pyproj4.github.io/pyproj/stable/gotchas.html#upgrading-to-pyproj-2-from-pyproj-1
      xy = transform(Proj(init='epsg:5181'), Proj(init='epsg:4326'), A_09['x'][0], A_09['y'][0])
    <ipython-input-9-65cd4d56332c>:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      A_09['lat'] = xy[1]
    <ipython-input-9-65cd4d56332c>:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      A_09['lng'] = xy[0]
    




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
</style>
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
</div>




```python
# 지도에 표시

m = folium.Map([A_09['lat'][0], A_09['lng'][0]], zoom_start=15)
folium.Marker([A_09['lat'][0], A_09['lng'][0]], popup=A_09['spot_nm'][0]).add_to(m)
m
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=%3C%21DOCTYPE%20html%3E%0A%3Chead%3E%20%20%20%20%0A%20%20%20%20%3Cmeta%20http-equiv%3D%22content-type%22%20content%3D%22text/html%3B%20charset%3DUTF-8%22%20/%3E%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%3Cscript%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20L_NO_TOUCH%20%3D%20false%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20L_DISABLE_3D%20%3D%20false%3B%0A%20%20%20%20%20%20%20%20%3C/script%3E%0A%20%20%20%20%0A%20%20%20%20%3Cstyle%3Ehtml%2C%20body%20%7Bwidth%3A%20100%25%3Bheight%3A%20100%25%3Bmargin%3A%200%3Bpadding%3A%200%3B%7D%3C/style%3E%0A%20%20%20%20%3Cstyle%3E%23map%20%7Bposition%3Aabsolute%3Btop%3A0%3Bbottom%3A0%3Bright%3A0%3Bleft%3A0%3B%7D%3C/style%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.6.0/dist/leaflet.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//code.jquery.com/jquery-1.12.4.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js%22%3E%3C/script%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.6.0/dist/leaflet.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdn.jsdelivr.net/gh/python-visualization/folium/folium/templates/leaflet.awesome.rotate.min.css%22/%3E%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cmeta%20name%3D%22viewport%22%20content%3D%22width%3Ddevice-width%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20initial-scale%3D1.0%2C%20maximum-scale%3D1.0%2C%20user-scalable%3Dno%22%20/%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cstyle%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%23map_115905b64da046b6bd553d576f896908%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20position%3A%20relative%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20width%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20height%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20left%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20top%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%3C/style%3E%0A%20%20%20%20%20%20%20%20%0A%3C/head%3E%0A%3Cbody%3E%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cdiv%20class%3D%22folium-map%22%20id%3D%22map_115905b64da046b6bd553d576f896908%22%20%3E%3C/div%3E%0A%20%20%20%20%20%20%20%20%0A%3C/body%3E%0A%3Cscript%3E%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20map_115905b64da046b6bd553d576f896908%20%3D%20L.map%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22map_115905b64da046b6bd553d576f896908%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20center%3A%20%5B37.565463500004306%2C%20127.02094049999522%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20crs%3A%20L.CRS.EPSG3857%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20zoom%3A%2015%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20zoomControl%3A%20true%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20preferCanvas%3A%20false%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20tile_layer_a2bb7edeea784d8f812331971caa9cee%20%3D%20L.tileLayer%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22https%3A//%7Bs%7D.tile.openstreetmap.org/%7Bz%7D/%7Bx%7D/%7By%7D.png%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%22attribution%22%3A%20%22Data%20by%20%5Cu0026copy%3B%20%5Cu003ca%20href%3D%5C%22http%3A//openstreetmap.org%5C%22%5Cu003eOpenStreetMap%5Cu003c/a%5Cu003e%2C%20under%20%5Cu003ca%20href%3D%5C%22http%3A//www.openstreetmap.org/copyright%5C%22%5Cu003eODbL%5Cu003c/a%5Cu003e.%22%2C%20%22detectRetina%22%3A%20false%2C%20%22maxNativeZoom%22%3A%2018%2C%20%22maxZoom%22%3A%2018%2C%20%22minZoom%22%3A%200%2C%20%22noWrap%22%3A%20false%2C%20%22opacity%22%3A%201%2C%20%22subdomains%22%3A%20%22abc%22%2C%20%22tms%22%3A%20false%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_115905b64da046b6bd553d576f896908%29%3B%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20marker_d991db45ac0e43919e2e982817baf41a%20%3D%20L.marker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B37.565463500004306%2C%20127.02094049999522%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_115905b64da046b6bd553d576f896908%29%3B%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20var%20popup_d4bea842910243ea8107a881cbbb278e%20%3D%20L.popup%28%7B%22maxWidth%22%3A%20%22100%25%22%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20html_722f725f78e64b01989c0417a4e08f98%20%3D%20%24%28%60%3Cdiv%20id%3D%22html_722f725f78e64b01989c0417a4e08f98%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%ED%87%B4%EA%B3%84%EB%A1%9C%28%EC%8B%A0%EB%8B%B9%EC%97%AD%29%3C/div%3E%60%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20popup_d4bea842910243ea8107a881cbbb278e.setContent%28html_722f725f78e64b01989c0417a4e08f98%29%3B%0A%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20marker_d991db45ac0e43919e2e982817baf41a.bindPopup%28popup_d4bea842910243ea8107a881cbbb278e%29%0A%20%20%20%20%20%20%20%20%3B%0A%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%3C/script%3E onload="this.contentDocument.open();this.contentDocument.write(    decodeURIComponent(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



# 교통류 데이터 분석


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
</style>
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
</div>




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
</style>
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
</div>




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
</style>
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
</div>




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
</style>
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
</div>




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
</style>
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
</div>




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
# 분석 대상 입력받기 (지금 주석처리)
"""
Day = input("분석 요일(평일, 주말, 일요일, 전체 중 입력) : ")

To = input("분석 방향(유입, 유출, 양방 중 입력) : ")

Tm = input("분석 시간 단위(분) : ")
Tm = Tm + 'T'

print("\n확인)", Day, To, Tm[:-1],"분")
"""
```




    '\nDay = input("분석 요일(평일, 주말, 일요일, 전체 중 입력) : ")\n\nTo = input("분석 방향(유입, 유출, 양방 중 입력) : ")\n\nTm = input("분석 시간 단위(분) : ")\nTm = Tm + \'T\'\n\nprint("\n확인)", Day, To, Tm[:-1],"분")\n'




```python
# 분석 모두 한번에 (지금 주석처리)
"""
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
"""
```




    "\nDl = ['평일', '주말', '일요일', '전체']\nTol = ['유입', '유출', '양방']\nTml = ['1T', '2T', '5T', '15T']\n\nfor i in range(len(Dl)):\n    Day = Dl[i]\n    \n    for j in range(len(Tol)):\n        To = Tol[j]\n        \n        for z in range(len(Tml)):\n            Tm = Tml[z]\n            \n            df_sample(Tm)\n            Q_K(df_result)\n            U_K(df_result)\n"




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


# Fundamental Diagram


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

    <ipython-input-28-7a2da2c73e5d>:54: RuntimeWarning: More than 20 figures have been opened. Figures created through the pyplot interface (`matplotlib.pyplot.figure`) are retained until explicitly closed and may consume too much memory. (To control this warning, see the rcParam `figure.max_open_warning`).
      plt.figure(figsize=(36,72))
    


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



```python

```
