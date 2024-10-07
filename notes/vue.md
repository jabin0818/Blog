## 封装组件事项

 

vue2可以使用is来渲染jsx组件，可以参考bkui-vue2-staging中的bk-tab-label

```js
export default {
  name: 'bk-tab-label',
  props: ['panel', 'index', 'closable', 'isSidePosition'],
  render (h) {
    const { renderLabel, $slots, label, name } = this.panel
    const labelContent = (renderLabel && renderLabel(h, name)) || $slots.label || label
    return (
      <li
        on-click={() => this.$parent.togglePanel(this.panel)}
        class={this.showClose && !this.isSidePosition ? 'has-close' : ''}>
        <div class="bk-tab-label">{ labelContent }</div>
        {
          this.showClose
            ? (<i class="bk-tab-close-controller" on-click={$event => this.handleClosePanel($event)}></i>)
            : ''
        }
      </li>
    )
  },
  computed: {
    showClose () {
      if (this.panel.disabled) {
        return false
      }
      if (this.panel.closable !== undefined) {
        return this.panel.closable
      }
      return this.closable
    }
  },
  methods: {
    handleClosePanel (event) {
      event.stopPropagation()
      this.$parent.handleClosePanel(this.index, this.panel)
    }
  }
}

```

渲染时引入:

 ```
<li is="bk-tab-label" ...></li>
import bkTabLabel from './bk-tab-label'
components: {
    bkTabLabel
},
 ```

### 变量恢复初始值，直接使用data声明的初始值

this.metricList=this.$options.data().metricList