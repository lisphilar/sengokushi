# 臣従大名に城を配分

実験用のサンプルシナリオVer2.0改造版。

## 条件イベントCE_0545（最終行）

武将が多い一方で石高が少ない臣従大名に対し、主家が城を与える。

```Python
CE_0545 {
    Title		: "臣従大名に城を配分"
    PeriodStart	: NONE
    PeriodEnd	: NONE
    Logic		: AND
    RepeatType	: REPEATABLE
    SubRoutine	: NO
    LoopType	: BUSHOU
    LoopVariable	: "LB"
    Condition {
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "ShiroA", "#城(LB)"
        CONDITION_SHIRO_NOT_HOUI, NONE, NONE, VARIABLE, NONE, "", ""
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "DaimyouA", "#所属大名(LB)"
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "DaimyouB", "#城支配大名(ShiroA)"
        CONDITION_NOT_EQUAL, NONE, NONE, NONE, NONE, "DaimyouA", "0"
        CONDITION_NOT_EQUAL, NONE, NONE, NONE, NONE, "DaimyouB", "0"
        CONDITION_LARGE, NONE, NONE, NONE, NONE, "#直轄城数(DaimyouB)", "1"
        CONDITION_SHINJUU_SPECIFIC, VARIABLE, VARIABLE, NONE, NONE, "", ""
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "KokudakaAPre", "#直轄石高(DaimyouA)"
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "KokudakaBPre", "#直轄石高(DaimyouB)"
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "NumberA", "#大名家直属武将数(DaimyouA)"
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "NumberB", "#大名家直属武将数(DaimyouB)"
        CONDITION_LARGE, NONE, NONE, NONE, NONE, "KokudakaBPre", "0"
        CONDITION_LARGE, NONE, NONE, NONE, NONE, "NumberB", "0"
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "KRatioPre", "KokudakaAPre/KokudakaBPre"
        CONDITION_DAINYUU, NONE, NONE, NONE, NONE, "BushouRatio", "NumberA/NumberB"
        CONDITION_SMALL, NONE, NONE, NONE, NONE, "KRatioPre", "BushouRatio"
    }
    Message {
        "%大名DaimyouA%の石高が武将数に対して不足していたため、"
        "%大名DaimyouA%に対して%大名DaimyouB%が%城Shiro1%（%SKokudaka%石）を加増しました。"
        ""
        "【武将数の比率】"
        "（%大名DaimyouA%の武将数）/（%大名DaimyouB%の武将数）=%NumberA%/%NumberB%=%BushouRatio%"
        ""
        "【石高の比率（変更前）】"
        "（%大名DaimyouA%の石高）/（%大名DaimyouB%の石高）=%KokudakaAPre%/%KokudakaBPre%=%KRatioPre%"
        ""
        "【石高の比率（変更後）】"
        "（%大名DaimyouA%の石高）/（%大名DaimyouB%の石高）=%KokudakaAPost%/%KokudakaBPost%=%KRatioPost%"
    }
    Happening {
        HAPPENING_DAINYUU, NONE, NONE, NONE, NONE, NONE, NONE, "Daimyou1", "DaimyouA"
        HAPPENING_DAINYUU, NONE, NONE, NONE, NONE, NONE, NONE, "Shiro1", "ShiroA"
        HAPPENING_SHIRO_SHOZOKU_CHANGE, VARIABLE, NONE, VARIABLE, NONE, NONE, NONE, "", ""
        HAPPENING_DAINYUU, NONE, NONE, NONE, NONE, NONE, NONE, "KokudakaAPost", "#直轄石高(DaimyouA)"
        HAPPENING_DAINYUU, NONE, NONE, NONE, NONE, NONE, NONE, "KokudakaBPost", "#直轄石高(DaimyouB)"
        HAPPENING_DAINYUU, NONE, NONE, NONE, NONE, NONE, NONE, "KRatioPost", "KokudakaAPost/KokudakaBPost"
        HAPPENING_DAINYUU, NONE, NONE, NONE, NONE, NONE, NONE, "SKokudaka", "KokudakaAPost-KokudakaAPre"
    }
}
```