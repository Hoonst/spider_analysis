# spider_analysis
Analysis on SPIDER, Benchmark Dataset of Text-to-SQL

해당 분석은 SPIDER Dataset에 대한 것이며, RAT-SQL 의 Prediction을 기반으로 수행되었습니다. 
기본 전처리 및 Data Split은 RAT-SQL에서 활용하는 방식을 사용하였습니다. 

### SPIDER Dataset의 난이도 분류 기준
[RAT-SQL 내 SPIDER Hardness Evaluation](https://github.com/microsoft/rat-sql/blob/051e7d35f3092d2c75b64dc0c7f1d791942d4f19/ratsql/datasets/spider_lib/evaluation.py#L387)

* SPIDER는 쿼리의 복잡도에 따라 난이도가 높아지게 되며, 복잡도의 기준은 SQL 구성 요소가 증가하는 것으로 측정한다.
* 즉, 기존 SPIDER Dataset에서 미리 난이도로 분류해놓은 데이터 셋이 아니라,
각 모델에서 Dev Set의 데이터 하나씩 검증해가며 평가한다.

```
def eval_hardness(self, sql):
    count_comp1_ = count_component1(sql)
    count_comp2_ = count_component2(sql)
    count_others_ = count_others(sql)

    if count_comp1_ <= 1 and count_others_ == 0 and count_comp2_ == 0:
        return "easy"
    elif (count_others_ <= 2 and count_comp1_ <= 1 and count_comp2_ == 0) or \
            (count_comp1_ <= 2 and count_others_ < 2 and count_comp2_ == 0):
        return "medium"
    elif (count_others_ > 2 and count_comp1_ <= 2 and count_comp2_ == 0) or \
            (2 < count_comp1_ <= 3 and count_others_ <= 2 and count_comp2_ == 0) or \
            (count_comp1_ <= 1 and count_others_ == 0 and count_comp2_ <= 1):
        return "hard"
    else:
        return "extra"
```

### SPIDER Dataset 난이도 비율 조사
* Dev Set: 1034
{'easy': 248, 'medium': 446, 'hard': 174, 'extra: 166}

Easy Accuracy: 0.7741935483870968
Medium Accuracy: 0.547085201793722
Hard Accuracy: 0.5114942528735632
Extra Accuracy: 0.3433734939759036

난이도가 증가 시, Exact Set Match 성능은 반비례하여 감소한다.