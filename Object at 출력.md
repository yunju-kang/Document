# Object at 출력

[문제]

```python
titanic_groupby = titanic_df.groupby('Pclass')
print(titanic_groupby)
```

```
<pandas.core.groupby.generic.DataFrameGroupBy object at 0x000001F2038B85E0>
```

`titanic_groupby`라는 변수를 출력하면 위와 같이 `object at ~~~`으로 출력된다.



[해결방안]

```python
titanic_groupby = pd.DataFrame(titanic_df.groupby('Pclass'))
print(titanic_groupby)
```

```python
   0                                                  1
0  1       PassengerId  Survived  Pclass  ...     Fa...
1  2       PassengerId  Survived  Pclass  ...     Fa...
2  3       PassengerId  Survived  Pclass  ...     Fa...
```

변수에 값은 존재하지만, `print()`가 이 변수값을 어떻게 출력해야하는지 모르는 경우 위와 같은 값이 반환된다.

`str`, `repr`, `dp.DataFrame`처럼 데이터타입을 설정해주면 변수값이 출력된다.