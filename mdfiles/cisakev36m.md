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
            plotColorPalette: '#36A2EB33, #ffd800'
            xAxisLabelColor: "#e1e1e1"
            xAxisTitleColor: "#e1e1e1"
            xAxisTickColor: "#e1e1e1"
            xAxisLineColor: "#e1e1e1"


---
xychart
    title "KEV Distribution vs Mean"
    x-axis "Monthly Distribution" [Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec]
    y-axis "KEV"  0 --> 250
    bar "month" [28, 32, 132, 164, 241, 160, 70, 59, 69, 73, 134, 40]
    line "mean" [100.16, 100.16, 100.16 "🚨", 100.16 "🚨", 100.16 "🚨", 100.16 "🚨", 100.16, 100.16 "Mean = 100.16", 100.16, 100.16, 100.16 "🚨", 100.16]

```

