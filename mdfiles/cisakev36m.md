# CISA KEV (36M)

```mermaid
---
config:
    xyChart:
        width: 900
        height: 600
        showDataLabel: true
        showDataLabelOutsideBar: true
    themeVariables:
        xyChart:
            titleColor: "#ffd800"
            plotColorPalette: '#36A2EB33, #ffd800'

---
xychart
    title "KEV Distribution vs Mean"
    x-axis "Monthly Distribution" [Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec]
    y-axis "KEV" 0 --> 250
    bar [28, 32, 132, 164, 241, 160, 70, 59, 69, 73, 134, 40]
    line [100.16, 100.16, 100.16, 100.16, 100.16, 100.16, 100.16, 100.16 "Mean = 100.16", 100.16, 100.16, 100.16, 100.16]

```

---
config:
  xychart:
    showDataLabel: true
    showDataLabelOutsideBar: true
  themeVariables:
    xyChart:
      plotColorPalette: '#0000FF, #0000FF, #FF0000, #FF0000, #FF0000, #FF0000, #0000FF, #0000FF, #0000FF, #0000FF, #FF0000, #0000FF'
---
