# 📊 DAX Measures Documentation

This document outlines the key DAX measures implemented in **IPL Analytics Intelligence Hub**.

The measures are grouped into:

* Match Intelligence
* Batting Metrics
* Bowling Metrics
* Phase Analytics
* Strategic Metrics
* Franchise Intelligence

All calculations were implemented inside Power BI using DAX.

# 🏏 Match Intelligence

## Total Matches

```DAX
Total Matches =
COUNTROWS(ipl_matches)
```

## Total Runs

```DAX
Total Runs =
SUM(ipl_deliveries[total_runs])
```

## Total Wickets

```DAX
Total Wickets =
SUM(ipl_deliveries[wicket_flag])
```

## Total Deliveries

```DAX
Total Deliveries =
COUNTROWS(ipl_deliveries)
```

# 📈 Batting Metrics

## Player Career Runs

```DAX
Player Career Runs =
SUM(ipl_deliveries[runs_batter])
```

## Player Strike Rate

Calculates strike rate based on runs scored per ball faced.

```DAX
Player Strike Rate =
DIVIDE(
    SUM(ipl_deliveries[runs_batter]),
    COUNTROWS(ipl_deliveries),
    0
) * 100
```

# 🎯 Bowling Metrics

## Player Career Wickets

Calculates wickets taken by a bowler using the inactive relationship between deliveries and players.

```DAX
Player Career Wickets =
CALCULATE(
    SUM(ipl_deliveries[wicket_flag]),
    USERELATIONSHIP(ipl_deliveries[bowler], Dim_Player[player])
)
```

# 📊 Phase Analytics

## Powerplay Runs

Runs scored during Powerplay overs.

```DAX
Powerplay Runs =
CALCULATE(
    SUM(ipl_deliveries[total_runs]),
    ipl_deliveries[over] <= 6
)
```

## Powerplay Balls

Total balls bowled in Powerplay.

```DAX
Powerplay Balls =
CALCULATE(
    COUNT(ipl_deliveries[ball]),
    ipl_deliveries[over] <= 6
)
```

## Powerplay Run Rate

```DAX
Powerplay Run Rate =
DIVIDE(
    [Powerplay Runs],
    [Powerplay Balls],
    0
) * 6
```

## Death Over Runs

Runs scored during death overs.

```DAX
Death Over Runs =
CALCULATE(
    SUM(ipl_deliveries[total_runs]),
    ipl_deliveries[over] >= 16
)
```

## Death Over Balls

```DAX
Death Over Balls =
CALCULATE(
    COUNT(ipl_deliveries[ball]),
    ipl_deliveries[over] >= 16
)
```

## Death Over Run Rate

```DAX
Death Over Run Rate =
DIVIDE(
    [Death Over Runs],
    [Death Over Balls],
    0
) * 6
```

# 🧠 Strategic Metrics

## Toss Wins

```DAX
Toss Wins =
CALCULATE(
    COUNTROWS(ipl_matches),
    ipl_matches[toss_winner] = ipl_matches[winner]
)
```

## Toss Impact %

Measures the percentage of matches won by the toss-winning team.

```DAX
Toss Impact % =
DIVIDE(
    [Toss Wins],
    COUNTROWS(ipl_matches),
    0
) * 100
```

## Total Chases

```DAX
Total Chases =
CALCULATE(
    COUNTROWS(ipl_matches),
    ipl_matches[toss_decision] = "field"
)
```

## Chase Wins

```DAX
Chase Wins =
CALCULATE(
    COUNTROWS(ipl_matches),
    ipl_matches[toss_decision] = "field",
    ipl_matches[toss_winner] = ipl_matches[winner]
)
```

## Chase Success %

```DAX
Chase Success % =
DIVIDE(
    [Chase Wins],
    [Total Chases],
    0
) * 100
```

# 🏆 Franchise Intelligence

## Total Wins

```DAX
Total Wins =
COUNTROWS(
    FILTER(
        ipl_matches,
        ipl_matches[winner] = SELECTEDVALUE(Dim_Team[team])
    )
)
```

## Total Seasons

```DAX
Total Seasons =
DISTINCTCOUNT(ipl_matches[season])
```

## Win Percentage

```DAX
Win Percentage =
DIVIDE(
    [Total Wins],
    [Total Matches],
    0
) * 100
```

# 🏆 Championship Metrics

## IPL Titles

```DAX
IPL Titles =
VAR SelectedTeam = SELECTEDVALUE(Dim_Team[team])

VAR Finals =
    FILTER(
        ipl_matches,
        ipl_matches[date] =
            CALCULATE(
                MAX(ipl_matches[date]),
                ALLEXCEPT(ipl_matches, ipl_matches[season])
            )
    )

RETURN
COUNTROWS(
    FILTER(
        Finals,
        ipl_matches[winner] = SelectedTeam
    )
)
```

## IPL Title Years

```DAX
IPL Title Years =
VAR SelectedTeam = SELECTEDVALUE(Dim_Team[team])

VAR Finals =
    FILTER(
        ipl_matches,
        ipl_matches[date] =
            CALCULATE(
                MAX(ipl_matches[date]),
                ALLEXCEPT(ipl_matches, ipl_matches[season])
            )
    )

RETURN
CONCATENATEX(
    FILTER(
        Finals,
        ipl_matches[winner] = SelectedTeam
    ),
    ipl_matches[season],
    ", ",
    ipl_matches[season],
    ASC
)
```

# 🧩 Modeling Notes

The data model follows a **Star Schema design**.

### Fact Tables

* IPL Deliveries
* IPL Matches
* Match Summary
* Over Summary

### Dimension Tables

* Teams
* Players
* Seasons
* Venues

The model is optimized for:

* Cross-filtering
* Drill-down analytics
* Match-level exploration
* Player performance analysis
* Strategic insights

# 📝 Summary

The DAX layer enables:

* Match-level analytics
* Player performance tracking
* Phase scoring analysis
* Toss and chase strategy insights
* Franchise performance analysis
* Championship history tracking

This completes the analytical layer of the **IPL Analytics Intelligence Hub** dashboard.