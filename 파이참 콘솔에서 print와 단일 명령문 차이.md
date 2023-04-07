### 파이참 콘솔에서의 print와 변수출력문

#### print문

앞과 뒤에 다른 코드가 있던 없던, print()문이 하나던 여러개건 상관없이 모든 print() 값이 In 섹션에서 출력됨.

- 단독으로 쓰이는 경우

```python
In[9]: print(type(titanic_pclass))
<class 'pandas.core.series.Series'>
```

- 중복으로 쓰이는 경우

```python
In[12]: print(type(titanic_pclass))
   ...: print(titanic_pclass.head())
```

```
<class 'pandas.core.series.Series'>
0    3
1    1
2    3
3    1
4    3
Name: Pclass, dtype: int64
```

- 뒤에 다른 코드가 있는 경우

```python
In[17]: print(type(titanic_pclass))
   ...: titanic_pclass.head()
```

```python
<class 'pandas.core.series.Series'>

Out[19]: 
0    3
1    1
2    3
3    1
4    3
Name: Pclass, dtype: int64
```

- 앞에 다른 코드가 있는 경우

```python
In[7]: type(titanic_pclass)
  ...: print(titanic_pclass.head())
```

```python
0    3
1    1
2    3
3    1
4    3
Name: Pclass, dtype: int64
```



#### 변수출력문

일반적으로 Out 섹션에서 값이 출력됨.

- 단독으로 사용하였을 경우

```python
In[10]: type(titanic_pclass)
```

```python
Out[10]: pandas.core.series.Series
```

- 앞에 다른 코드가 있을 경우

```python
In[17]: print(type(titanic_pclass))
   ...: titanic_pclass.head()
```

```python
<class 'pandas.core.series.Series'>

Out[19]: 
0    3
1    1
2    3
3    1
4    3
Name: Pclass, dtype: int64
```



복수의 변수출력문이 사용될 경우 마지막 변수출력문만 출력됨.

- 중복으로 사용하였을 경우

```python
In[17]: type(titanic_pclass)
   ...: titanic_pclass.head()
```

```python
Out[17]: 
0    3
1    1
2    3
3    1
4    3
Name: Pclass, dtype: int64
```



뒤에 다른 명령어가 있는 경우 변수출력문이 무시됨.

- 뒤에 다른 명령어가 쓰인 경우

```python
In[7]: type(titanic_pclass)
  ...: print(titanic_pclass.head())
```

```python
0    3
1    1
2    3
3    1
4    3
Name: Pclass, dtype: int64
```