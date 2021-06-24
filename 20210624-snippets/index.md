# 代码片段


#### 模拟点击
```
const btn = document.getElementById('demo_click')
function simulateClick() {
    const event = new MouseEvent('click', {
        view: window,
        bubbles: true,
        cancelable: true
    });
    btn.dispatchEvent(event);      
    }

```


