# CISA KEV (36M)

```mermaid
---
config:
    xyChart:
        width: 640
        height: 480
        showLegend: true
        showDataLabel: true
        showDataLabelOutsideBar: true
    themeVariables:
        xyChart:
            backgroundColor: "#1e1e1e"
            titleColor: "#ffd800"
            dataLabelColor: "#e1e1e1"
            xAxisLabelColor: "#e1e1e1"
            xAxisTitleColor: "#e1e1e1"
            xAxisTickColor: "#e1e1e1"
            xAxisLineColor: "#e1e1e1"
            yAxisLabelColor: "#e1e1e1"
            yAxisTitleColor: "#e1e1e1"
            yAxisTickColor: "#e1e1e1"
            yAxisLineColor: "#e1e1e1"
            plotColorPalette: '#ffd800, #36A2EB33, #FF638433'


---
xychart
    title "KEV Distribution vs Mean"
    x-axis "Monthly Distribution" [Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec]
    y-axis "KEV"  0 --> 250
    line "mean" [100.16, 100.16, 100.16 "🚨", 100.16 "🚨", 100.16 "🚨", 100.16 "🚨", 100.16, 100.16 "Mean = 100.16", 100.16, 100.16, 100.16 "🚨", 100.16]
    bar "below" [28, 32, 0, 0, 0, 0, 70, 59, 69, 73, 0, 40]
    bar "above" [0, 0, 132, 164, 241, 160, 0, 0, 0, 0, 134, 0]

```

