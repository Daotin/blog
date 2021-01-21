# 73 javascript-防止 window.open 被拦截

```javascript
window.open('/app/dashbuilder.html?' + group.id, '_blank'); // 一般_self不会被拦截

// 改为
let newTab = window.open('about:blank', '_blank');
newTab.location.href = "/app/dashbuilder.html?" + group.id;
```

