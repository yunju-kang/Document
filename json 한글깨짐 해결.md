# json 한글깨짐 해결	

[문제]

```python
import json

# json파일 읽어오기
with open('파일경로', 'r') as file:
  data = json.load(file)
  
# json.dump로 데이터 저장  
result = json.dumps(data)

# 결과 출력
print(result)
```

 json 파일을 읽어와 json.dumps를 하면 한글일 깨져보임

```
{"\uc81c\uc8fc\uc2dc": ["\uac74\uc785\ub3d9", "\uad6c\uc88c\uc74d", ......}
```



[해결방법]

```
import json

# json파일 읽어오기
with open('파일경로', 'r') as file:
  data = json.load(file)
  
# json.dump로 데이터 저장  
result = json.dumps(region, indent=4, ensure_ascii=False)

# 결과 출력
print(result)
```

  json.dumps시 "ensure_ascii=False" 설정 추가

```
{'제주시': ['건입동', '구좌읍', '내도동', '노형동', '도남동', '도두동', ......}
```

