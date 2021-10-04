# 티맵 데이터 전처리

## 데이터 형식 수정, 좌표, 주행 시간, 거리 계산 및 이상치 제거


```python
!pip install shapely
!pip install --upgrade setuptools
!pip install geopandas
!pip install os
```


```python
import os
import datetime
import pandas as pd
from pyproj import Proj, transform  
from scipy.spatial import distance
from shapely.geometry import Point, Polygon
```


```python
df=pd.read_csv("C:/Users/smartmobility/Desktop/Work/navi/좌표변환.csv",encoding="euc-kr")

#자르고 싶은 기준 도시
df=df[df["도시"]=="통영"]
#이 밑에 path는 슬라이싱해서 드린 데이터만 들어있는 곳이여야합니다.

path = "C:/Users/smartmobility/Desktop/Work/통영_MATSim/맵매칭/전처리2/"
file_list = os.listdir(path)

#빈데이터프레임 생성
#data=pd.read_csv(path+"test_102725.csv")
data=pd.DataFrame(index=range(0),columns=['obu_id','date_umd','date_time','x1','y1','sp1','x2','y2','sp2','x3','y3','sp3','x4','y4','sp4','x5','y5','sp5','x6','y6','sp6','x7','y7','sp7','x8','y8','sp8','x9','y9','sp9','x10','y10','sp10','x11','y11','sp11','x12','y12','sp12','x13','y13','sp13','x14','y14','sp14','x15','y15','sp15','x16','y16','sp16','x17','y17','sp17','x18','y18','sp18','x19','y19','sp19','flag','node_id'])

for i in range(0,0):
    navi=pd.read_csv(path+i,encoding="ANSI",error_bad_lines=False)
    navi=navi.apply(pd.to_numeric,errors="coerce")
    navi=navi.drop(navi[navi["x1"]==0].index)
    navi=navi.drop(navi[navi["y1"]==0].index)
    navi["x1"]=pd.to_numeric(navi["x1"],errors="coerce")
    navi["y1"]=pd.to_numeric(navi["y1"],errors="coerce")
    navi.fillna(0,inplace=True)
    navi=navi.drop(navi[navi["date_umd"]< 20170000].index)
    navi.reset_index(inplace=True,drop=True)
    navi_2=navi[navi["y1"]<int(df[df["방위"]=="north"]["lat"])]
    navi_2=navi_2[navi_2["y1"]>int(df[df["방위"]=="south"]["lat"])]
    navi_2=navi_2[navi_2["x1"]>int(df[df["방위"]=="west"]["long"])]
    navi_2=navi_2[navi_2["x1"]<int(df[df["방위"]=="east"]["long"])]
    
    navi_3=pd.merge(navi,navi_2["obu_id"],on='obu_id')
    #중복된 행 제거
    navi_3.drop_duplicates(inplace=True)
    # concat을 통해서 중복된 행에 붙이는 작업
    data=pd.concat([data,navi_3])
data.reset_index(inplace=True, drop=True)
```


```python
path = "C:/Users/smartmobility/Desktop/Work/통영_MATSim/맵매칭/전처리2/"
data=pd.read_csv(path+"full_data_3.csv")
```


```python
def Navi_2(df):
        df_3=pd.DataFrame(columns=["x","y","time","time_difference","구분","거리(m)"])
        if len(df)==0:
            
            return df_3
        
        df.sort_values(by=["date_time"],inplace=True)
        df.reset_index(drop=True,inplace=True)
        if df.isnull().sum().sum()>0:
            df.fillna(0,inplace=True)
        
        xcol=[c for c in df.columns if c.lower()[:1] == 'x']
        ycol=[c for c in df.columns if c.lower()[:1] == 'y']
        try:
            df=df.apply(pd.to_numeric)
        except :
            return df_3

        df_x=df[xcol]
        df_y=df[ycol]
        df_time=df["date_time"]
        df_x=df_x.cumsum(axis=1)
        df_y=df_y.cumsum(axis=1)
        df_date=df["date_umd"]
       
        
        for i in range(len(df_time)):
            
            t=str(int(df_date[i]))+str(int(df_time[i]))
            df_time[i]=datetime.datetime.strptime(str(t),"%Y%m%d%H%M%S")
           
            
        df_1=pd.DataFrame(columns=["x","y","time"])
        pre_time=df_time[0]
        for i in range(len(df_x)):
            time=df_time[i]
            pre_time=minusSecs_3(time)
            for j in range(len(df_x.columns)):
                time=addSecs_3(pre_time)
                
                x=df_x.iloc[i,j]
                y=df_y.iloc[i,j]
                pre_time=time
                df_1.loc[len(df_1)+1]=[x,y,time]
        #시간 순으로 정렬        
        df_1.sort_values(by=["time"],inplace=True)
        df_1.reset_index(inplace=True)
        post_time=list(df_1["time"])
        del post_time[-1]
        
        post_time.insert(0,post_time[0])
        df_1["post_time"]=post_time
        
   
        df_1["time_difference"]=0
        for i in range(len(df_1)):
            df_1["time_difference"][i]=minusSecs(df_1["time"][i],df_1["post_time"][i])
        
        del df_1["post_time"]
        

        count=1
        df_1["구분"]=0
        for i in range(0,len(df_1)):
            if df_1["time_difference"][i]>datetime.timedelta(hours=1):
                count+=1
                df_1["구분"][i]=count
         
            else:
                df_1["구분"][i]=count    
        
        df_2=df_1
        lst=list(set(df_2["구분"]))
        df_3=pd.DataFrame(columns=["x","y","time","time_difference","구분","거리(m)"])

        for j in lst:
            df_temp=df_2[df_2["구분"]==j]

            df_temp.reset_index(inplace=True)
            df_temp["거리(m)"]=0
            
            df_3=df_3.append(df_temp)
        df_3=df_3[["x","y","time","time_difference","거리(m)","구분"]]
    
        return df_3
   
        
        
def addSecs_3(tm):
    fulldate = datetime.datetime(tm.year, tm.month, tm.day, tm.hour, tm.minute, tm.second)
    fulldate = fulldate + datetime.timedelta(seconds=3)
    return fulldate

def minusSecs_3(tm):
    fulldate = datetime.datetime(tm.year, tm.month, tm.day, tm.hour, tm.minute, tm.second)
    fulldate = fulldate - datetime.timedelta(seconds=3)
    return fulldate

def minusSecs(tm,tm_2):
    fulldate = datetime.datetime(tm.year, tm.month, tm.day, tm.hour, tm.minute, tm.second)
    time_difference = fulldate - datetime.datetime(tm_2.year, tm_2.month, tm.day, tm_2.hour, tm_2.minute, tm_2.second)
    return time_difference

```


```python
print(len(data))
print(len(id_lst))
```


```python
import time
```

## 함수 제작 및 실행


```python
start=time.time()
final=pd.DataFrame(columns=["id","x","y","time"])
fail=[]
success=[]
id_lst=list(set(data["obu_id"]))
id_lst.sort(reverse=False)
for i in range(1426, len(id_lst)):
    tmp=data[data["obu_id"]==id_lst[i]]
    date=tmp["date_umd"]
    tmp=Navi_2(tmp)
    if len(tmp)==0:
        fail.append(id_lst[i])
        print("\n",i,"\t실패",id_lst[i], time.time()-start)
    else:
        success.append(id_lst[i])
        print("\n",i,"\t성공",id_lst[i], time.time()-start)
        tmp["id"]=id_lst[i]
        final=final.append(tmp)
    
    final_2=final.set_index(["id","구분"])
    final_2.to_csv(path+"final3.csv",encoding="ANSI")
```

## OD 구분


```python
#중간 이동은 뺴고 id와 구분을 기준으로 od파일을 제작하는 방법
final_ori=pd.DataFrame(columns=["id","x","y","time","time_difference","구분","거리(m)"])
final_des=pd.DataFrame(columns=["id","x","y","time","time_difference","구분","거리(m)"])
index_list=list(set(final_2.index))
for i in index_list:
        tmp=final_2.loc[i,:]
        tmp.reset_index(inplace=True)
        final_ori=final_ori.append(tmp.loc[0])
        final_des=final_des.append(tmp.loc[len(tmp)-1])
```


```python
final_ori.rename(columns = {'x' : 'start_x','y':'start_y','time':'start_time'}, inplace = True)
final_des.rename(columns = {'x' : 'end_x','y':'end_y','time':'end_time'}, inplace = True)
final_ori=final_ori[['id','start_x','start_y','start_time','구분']]
final_des=final_des[['id','end_x','end_y','end_time','구분']]
final_od=pd.merge(final_ori,final_des,how="outer")
```


```python
df_Tongyeong=pd.read_csv("C:/Users/smartmobility/Downloads/navi/춘천경계_2.csv",encoding="ANSi")
#필요한 컬럼만 자른 후
df_2=df_Tongyeong[["long","lat"]]
#리스트를 연결한 리스트를 만들기 위해서 apply로 각 행들을 리스트로 구성한 후 그것들을 tolist()로 연결
Tongyeong_2=df_2.apply(list,axis=1).tolist()
```


```python
poly=Polygon(Tongyeong_2)
final_od["시작_지역"]=""
final_od["도착_지역"]=""
for i in range(len(final_od)):
    p=Point(final_od["start_x"][i],final_od["start_y"][i])
    final_od["시작_지역"][i]=p.within(poly)
    p=Point(final_od["end_x"][i],final_od["end_y"][i])
    final_od["도착_지역"][i]=p.within(poly)
```


```python
final_od["통행구분"]=""
final_od.loc[(final_od["시작_지역"]==True) & (final_od["도착_지역"]==True),'통행구분']="내부통행"
final_od.loc[(final_od["시작_지역"]==True) & (final_od["도착_지역"]==False),'통행구분']="내부유출"
final_od.loc[(final_od["시작_지역"]==False) & (final_od["도착_지역"]==True),'통행구분']="내부유입"
final_od.loc[(final_od["시작_지역"]==False) & (final_od["도착_지역"]==False),'통행구분']="외부통행"
```


```python
final_od.to_csv("C:/Users/smartmobility/Downloads/navi/네비게이션정리_춘천_2.csv",encoding="ANSI",index=False)
```


```python
final_od
```

## 통행 구분


```python
od_id=final_od.loc[:, ['id', '구분', '통행구분']]
od_id
```


```python
final_3=pd.read_csv("C:/Users/smartmobility/Downloads/navi/네비게이션정리_춘천.csv",encoding="ANSi")
```


```python
od_data=pd.merge(od_id, final_3, how='left')
od_data
```


```python
od_유출=od_data[od_data["통행구분"]=="내부유출"]
od_유입=od_data[od_data["통행구분"]=="내부유입"]
od_내부=od_data[od_data["통행구분"]=="내부통행"]
od_외부=od_data[od_data["통행구분"]=="외부통행"]
```


```python
od_유출.to_csv("C:/Users/smartmobility/Downloads/navi/춘천_유출.csv",encoding="ANSI",index=False)
od_유입.to_csv("C:/Users/smartmobility/Downloads/navi/춘천_유입.csv",encoding="ANSI",index=False)
od_내부.to_csv("C:/Users/smartmobility/Downloads/navi/춘천_내부.csv",encoding="ANSI",index=False)
od_외부.to_csv("C:/Users/smartmobility/Downloads/navi/춘천_외부.csv",encoding="ANSI",index=False)
```


```python
od_id["통행구분"].value_counts()
```


```python
od_id["id"].value_counts()
```
