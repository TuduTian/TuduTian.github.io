# react 基础

##  router 路由 v6 版本 (路由懒加载)
```tsx
   const lazyLoad = (src: () => Promise<{ default: React.ComponentType<any>; }>) => <Suspense fallback={<>...</>}>{React.createElement(lazy(src))}</Suspense>;
  {
    path: '/',
    element: lazyLoad(() => import('../pages/Home')),
    children: [
      {
        index: true,
        element: lazyLoad(() => import('../pages/Children'))
      },
      {
        path:'/other',
        element:lazyLoad(()=>import('../pages/Other'))
      }
    ]
  },
```
> 路由要包裹在 BrowserRouter(hisrty路由，不带#号的)标签内 或者是 HashRouter(hash路由 带#号的)标签内

1.  嵌套路由的写法
> 跟vue不同的是，他没有重定向（redirect），在children数组中，你想让他默认显示哪一个就添加 index：true就可以了,上一个代码块有示例