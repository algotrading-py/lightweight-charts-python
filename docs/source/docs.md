# Docs

[![louisnw01 - lightweight-charts-python](https://img.shields.io/static/v1?label=louisnw01&message=lightweight-charts-python&color=057dfc&logo=github)](https://github.com/louisnw01/lightweight-charts-python "Go to GitHub repo")
[![PyPi Release](https://img.shields.io/pypi/v/lightweight-charts?color=32a852&label=PyPi)](https://pypi.org/project/lightweight-charts/)
[![Made with Python](https://img.shields.io/badge/Python-3.9+-c7a002?logo=python&logoColor=white)](https://python.org "Go to Python homepage")
[![License](https://img.shields.io/github/license/louisnw01/lightweight-charts-python?color=9c2400)](https://github.com/louisnw01/lightweight-charts-python/blob/main/LICENSE)
[![Stars - lightweight-charts-python](https://img.shields.io/github/stars/louisnw01/lightweight-charts-python?style=social)](https://github.com/louisnw01/lightweight-charts-python)
[![Forks - lightweight-charts-python](https://img.shields.io/github/forks/louisnw01/lightweight-charts-python?style=social)](https://github.com/louisnw01/lightweight-charts-python)


## Common Methods
These methods can be used within the `Chart`, `QtChart`, and `WxChart` objects.

___
### `set`
`data: pd.DataFrame`

Sets the initial data for the chart.

The data must be given as a DataFrame, with the columns:

`time | open | high | low | close | volume`

The `time` column can also be named `date`, and the `volume` column can be omitted if volume is not enabled.
___

### `update`
`series: pd.Series`

Updates the chart data from a given bar.

The bar should contain values with labels of the same name as the columns required for using `chart.set()`.
___

### `update_from_tick`
`series: pd.Series`

Updates the chart from a tick.

The series should use the labels:

`time | price | volume`

As before, the `time` can also be named `date`, and the `volume` can be omitted if volume is not enabled.

```{information}
The provided ticks do not need to be rounded to an interval (1 min, 5 min etc.), as the library handles this automatically.```````
```


___

### `create_line`
`color: str` | `width: int` | `-> Line`

Creates and returns a [Line](#line) object.
___

### `marker`
`time: datetime` | `position: 'above'/'below'/'inside'` | `shape: 'arrow_up'/'arrow_down'/'circle'/'square'` | `color: str` | `text: str` | `-> UUID`

Adds a marker to the chart, and returns its UUID.

If the `time` parameter is not given, the marker will be placed at the latest bar.
___

### `remove_marker`
`m_id: UUID`

Removes the marker with the given UUID.

Usage:
```python
marker = chart.marker(text='hello_world')
chart.remove_marker(marker)
```
___

### `horizontal_line`
`price: float/int` | `color: str` | `width: int` | `style: 'solid'/'dotted'/'dashed'/'large_dashed'/'sparse_dotted'` | `text: str` | `axis_label_visible: bool`

Places a horizontal line at the given price.
___

### `remove_horizontal_line`
`price: float/int`

Removes a horizontal line at the given price.
___

### `config`
`mode: 'normal'/'logarithmic'/'percentage'/'index100'` | `title: str` | `right_padding: int`

Config options for the chart.
___

### `time_scale`
`time_visible: bool` | `seconds_visible: bool`

Options for the time scale of the chart.
___

### `layout`
`background_color: str` | `text_color: str` | `font_size: int` | `font_family: str`

Global layout options for the chart.
___

### `candle_style`
`up_color: str` | `down_color: str` | `wick_enabled: bool` | `border_enabled: bool` | `border_up_color: str` | `border_down_color: str` | `wick_up_color: str` | `wick_down_color: str`

 Candle styling for each of the candle's parts (border, wick).

```{admonition} Color Formats
:class: note

Throughout the library, colors should be given as either: 
* rgb: `rgb(100, 100, 100)`
* rgba: `rgba(100, 100, 100, 0.7)`
* hex: `#32a852`
```
___

### `volume_config`
`scale_margin_top: float` | `scale_margin_bottom: float` | `up_color: str` | `down_color: str`

Volume config options.

```{important}
The float values given to scale the margins must be greater than 0 and less than 1.
```
___

### `crosshair`
`mode` | `vert_width: int` | `vert_color: str` | `vert_style: str` | `vert_label_background_color: str` | `horz_width: int` | `horz_color: str` | `horz_style: str` | `horz_label_background_color: str`

Crosshair formatting for its vertical and horizontal axes.

`vert_style` and `horz_style` should be given as one of: `'solid'/'dotted'/'dashed'/'large_dashed'/'sparse_dotted'`
___

### `watermark`
`text: str` | `font_size: int` | `color: str`

Overlays a watermark on top of the chart.
___

### `legend`
`visible: bool` | `ohlc: bool` | `percent: bool` | `color: str` | `font_size: int` | `font_family: str`

Configures the legend of the chart.
___

### `subscribe_click`
`function: object`

Subscribes the given function to a chart 'click' event.

The event emits a dictionary containing the bar at the time clicked, with the keys:

`time | open | high | low | close`
___

### `create_subchart`
`volume_enabled: bool` | `position: 'left'/'right'/'top'/'bottom'`, `width: float` | `height: float` | `sync: bool/UUID` | `-> SubChart`

Creates and returns a [SubChart](#subchart) object, placing it adjacent to the declaring `Chart` or `SubChart`.

`position`: specifies how the `SubChart` will float within the `Chart` window.

`height` | `width`: Specifies the size of the `SubChart`, where `1` is the width/height of the window (100%)

`sync`: If given as `True`, the `SubChart`'s time scale will follow that of the declaring `Chart` or `SubChart`. If a `UUID` object is passed, the `SubChart` will follow the panel with the given `UUID`.

Chart `UUID`'s  can be accessed from the`chart.id` and `subchart.id` attributes. 

```{important}
`width` and `height` must be given as a number between 0 and 1.
```

___


## `Chart`
`volume_enabled: bool` | `width: int` | `height: int` | `x: int` | `y: int` | `on_top: bool` | `debug: bool` 

The main object used for the normal functionality of lightweight-charts-python, built on the pywebview library.
___

### `show`
`block: bool`

Shows the chart window. If `block` is enabled, the method will block code execution until the window is closed.
___

### `hide`

Hides the chart window, and can be later shown by calling `chart.show()`.
___

### `exit`

Exits and destroys the chart and window.

___

## `Line`

The `Line` object represents a `LineSeries` object in Lightweight Charts and can be used to create indicators.

```{important}
The `line` object should only be accessed from the [create_line](#create-line) method of `Chart`.
```
___

### `set`
`data: pd.DataFrame`

Sets the data for the line.

This should be given as a DataFrame, with the columns: `time | value`
___

### `update`
`series: pd.Series`

Updates the data for the line.

This should be given as a Series object, with labels akin to the `line.set()` function.
___

## `SubChart`

The `SubChart` object allows for the use of multiple chart panels within the same `Chart` window. All of the [Common Methods](#common-methods) can be used within a `SubChart`.

`SubCharts` are arranged horizontally from left to right. When the available space is no longer sufficient, the subsequent `SubChart` will be positioned on a new row, starting from the left side.
___

### Grid of 4 Example:
```python
import pandas as pd
from lightweight_charts import Chart

if __name__ == '__main__':
    
    chart = Chart(inner_width=0.5, inner_height=0.5)

    chart2 = chart.create_subchart(position='right', width=0.5, height=0.5)

    chart3 = chart2.create_subchart(position='left', width=0.5, height=0.5)

    chart4 = chart3.create_subchart(position='right', width=0.5, height=0.5)

    chart.watermark('1')
    chart2.watermark('2')
    chart3.watermark('3')
    chart4.watermark('4')

    df = pd.read_csv('ohlcv.csv')
    chart.set(df)
    chart2.set(df)
    chart3.set(df)
    chart4.set(df)

    chart.show(block=True)

```
___

### Synced Line Chart Example:

```python
import pandas as pd
from lightweight_charts import Chart

if __name__ == '__main__':
 
    chart = Chart(inner_width=1, inner_height=0.8)
    
    chart2 = chart.create_subchart(width=1, height=0.2, sync=True, volume_enabled=False)
    chart2.time_scale(visible=False)
    
    df = pd.read_csv('ohlcv.csv')
    df2 = pd.read_csv('rsi.csv')
    
    chart.set(df)
    line = chart2.create_line()
    line.set(df2)
    
    chart.show(block=True)

```
___


## `QtChart`
`widget: QWidget` | `volume_enabled: bool`

The `QtChart` object allows the use of charts within a `QMainWindow` object, and has similar functionality to the `Chart` object for manipulating data, configuring and styling.
___

### `get_webview`

`-> QWebEngineView`

Returns the `QWebEngineView` object.

___
### Example:

```python
import pandas as pd
from PyQt5.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QWidget

from lightweight_charts.widgets import QtChart

app = QApplication([])
window = QMainWindow()
layout = QVBoxLayout()
widget = QWidget()
widget.setLayout(layout)

window.resize(800, 500)
layout.setContentsMargins(0, 0, 0, 0)

chart = QtChart(widget)

df = pd.read_csv('ohlcv.csv')
chart.set(df)

layout.addWidget(chart.get_webview())

window.setCentralWidget(widget)
window.show()

app.exec_()
```
___

## `WxChart`
`parent: wx.Panel` | `volume_enabled: bool`

The WxChart object allows the use of charts within a `wx.Frame` object, and has similar functionality to the `Chart` object for manipulating data, configuring and styling.

___

### `get_webview`
`-> wx.html2.WebView`

Returns a `wx.html2.WebView` object which can be used to for positioning and styling within wxPython.
___

### Example:

```python
import wx
import pandas as pd

from lightweight_charts.widgets import WxChart


class MyFrame(wx.Frame):
    def __init__(self):
        super().__init__(None)
        self.SetSize(1000, 500)

        panel = wx.Panel(self)
        sizer = wx.BoxSizer(wx.VERTICAL)
        panel.SetSizer(sizer)

        chart = WxChart(panel)

        df = pd.read_csv('ohlcv.csv')
        chart.set(df)

        sizer.Add(chart.get_webview(), 1, wx.EXPAND | wx.ALL)
        sizer.Layout()
        self.Show()


if __name__ == '__main__':
    app = wx.App()
    frame = MyFrame()
    app.MainLoop()

```


