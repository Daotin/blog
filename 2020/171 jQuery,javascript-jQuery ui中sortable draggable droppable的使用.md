最近工作中用到了jQuery UI中排序和拖拽功能，花了大概一天的时间，搞清楚了大概的参数配置，以及遇到的一些问题，总结如下。

## sortable
简单的配置如下：

```javascript
$('#subs-box').sortable({
    axis: 'y',
    cursor: 'ns-resize',
    placeholder: "ui-state-highlight", // 排序过程中占位符的class样式设置
    forcePlaceholderSize: true, // 强迫占位符有一个尺寸大小。
    handle:'.sort-at', // 在对象内指定的元素上开始拖动，而不是整个元素都可以拖动
    distance: 10,
    opacity: 0.8,
    containment：'parent', // 元素可以拖动排序的范围
    // helper: 'clone', // 是否clone一个元素进行拖动
    items: '.subject',  // 指定哪些元素可以排序
    stop: function (e, ui) {
        // 排序后元素的顺序（前提每个元素都需要有id属性）
        let newSubArr = $("#subs-box").sortable('toArray'); 
        console.log(newSubArr);
    },
}).disableSelection(); // 拖动时禁止选中元素
```
还有一些排序时候的事件和方法，都在参考链接的文档里面。


## draggable

```javascript
dragInit() {
    let me = this;
    let selector = '.ptype-'+me.selectedSubType;
    
    // 题目拖动
    $('#subs-box .subject').draggable({
        // appendTo: ".ptype-item.radio", // 当进行拖动时，拖动组件助手应该被添加到的元素。
        // connectToSortable: "#subs-box", // 允许draggable被拖拽到指定的sortables中。
        
        // 拖动时使用的是clone的元素。如果值设置为"clone", 那么该元素将会被复制，并且被复制的元素将被拖动。
        // 之所以不使用 helper: 'clone', 是因为clone的元素没有样式，所以我们需要自定义样式，所以使用了自定义函数。
        helper: function() {
            let helper = $(this).clone();
            helper.css({'width': $(this).width(), 'background': '#fff'}); // 设置clone元素的样式
            return helper;
        },
        handle: ".drag-at", // 指定在特定的元素上触发鼠标按下事件时，才可以拖动。
        cursor: 'move',
        // containment: '.sub-box', // 可以限制draggable只能在指定的元素或区域的边界以内进行拖动。
        revert: 'invalid', // 如果设置为true，当拖动停止时，元素位置将被重置。
        revertDuration: 200,
        distance: 10,
        opacity: 0.8,
        zIndex: 10000,
        refreshPositions: true, // 所有的可拖动位置在每次鼠标移动时都会被计算。（设置该值使得drop的位置更加精确）
        start(event, ui) {
            $(selector).addClass('allow'); // 元素拖拽的时候，设置可放置元素的样式，示意你可以拖拽到那里去
            // 开始拖拽的时候，初始化drop
            me.$nextTick(()=>{
                me.dropInit();
            });
        },
        stop(event, ui) {
            $(selector).removeClass('allow');
            // 拖拽停止的时候，销毁drop函数。
            me.dropDestory();
        }
    }).disableSelection();

},
```


> 注意事项：
>
>  **每次dropInit函数初始化后，如果需要再次初始化，需要先销毁之前的放置对象**。否则第一次初始化后，比如某个地方A可以放置拖拽的元素，但是第二次初始化后，地方A就不可以放置了。然而实际上，如果你不把第一次初始化的dropInit函数销毁掉，地方A在第二次初始化后还是可以放置的。所以需要在拖拽停止的时候，销毁上一次的dropInit对象。


## dropable

```javascript
dropInit() {
    let me = this;
    // 题目放置（设置题目根据不同类型可以放置不同的分页）
    // selector是可变的，也就是每次可拖拽元素可放置的元素是不同的。所以需要每次拖拽后清除之前dropInit对象。
    let selector = '.ptype-'+me.selectedSubType;

    $(selector).droppable({
        // accept: selector,
        // accept: function(d) {
        //     if($(this).hasClass('ptype'+me.selectedSubType)){
        //         console.log('d>>>>>>',$(this)[0]);
        //         return true;
        //     }
        // },
        // hoverClass: "drop-hover",
        tolerance: 'pointer', // 指定使用那种模式来测试一个拖动(draggable)元素"经过"一个放置（droppable）对象
        drop: function( event, ui ) {
            // $(this) 填充到的元素
            // ui.draggable.context 填充的元素
            let dragId = $(ui.draggable.context).attr('id');
            let dropId = $(this).attr('id');

            // 移动到新的分页
            if(dropId === 'newpage') {
                me.moveAddPage(dragId);
            } else { // 移动题目到另一个分页
                if(dropId === me.selectedPage.id) { // 移动到自己的分组，不做处理

                } else {
                    let index = me.selectedPage.subs.indexOf(dragId);
                    if(index > -1) {
                        me.selectedPage.subs.splice(index, 1);

                        me.pages.forEach(page=>{
                            if(page.id === dropId) {
                                page.subs.push(dragId);
                            }
                        });

                        me.$openNotice('移动成功');
                        
                        // 其他操作...
                    }
                }
            }

            $(this).removeClass('allow-hover');
        },
        over(event, ui) {
            $(this).addClass('allow-hover'); // 当拖拽元素进入可放元素时，可放置元素本身的样式
        },
        out() {
            $(this).removeClass('allow-hover'); // 设置拖拽元素离开可放元素时，清除可放置元素本身的样式
        }
    });
},
dropDestory() {
    let selector = '.ptype-'+me.selectedSubType;
    $(selector).droppable("destroy");
},
```



## 参考链接
- https://www.html.cn/jquery-ui-api/sortable/
- https://www.html.cn/jquery-ui-api/draggable/
- https://www.html.cn/jquery-ui-api/droppable