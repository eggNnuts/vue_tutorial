# Vue Life Cycle

> DOM 객체에 Vue Instance나 컴포넌트가 생성될때, 우리 눈에 보여지고 사라자기까지의 단계
> - vue methods를 new 생성자로 instance를 생성한 하면 아래 코드들의 해당 하는 methods를 확인 할수 있다  
> ```
> // vue cdn -> https://cdn.jsdelivr.net/npm/vue/dist/vue.js
>function Vue (options) {
>  if (process.env.NODE_ENV !== 'production' && !(this instanceof Vue)) {
>    warn('Vue is a constructor and should be called with the `new` keyword')
>  }
>  this._init(options)
>}
>```
> <br>


## 1. Create 
> Vue.js 의 라이플 사이클 중에 가장먼저 실행 
> create 단계에서는 실행되는 라이프 사이클 hook 들은 컴포넌트가 Dom에 추가 
> 되기 전이기 때문에 DOM에 접근(this.$el)을 사용할 수 없다. 
> Create단계에서 호출되는  Hook 은 beforeCreate와 created
>```
>Vue.prototype._init = function (options) {
>      var vm = this;
>      // a uid
>      vm._uid = uid$3++;
>      ....
>      vm._self = vm;
>      initLifecycle(vm);
>      initEvents(vm);
>      initRender(vm);
>      callHook(vm, 'beforeCreate');
>      initInjections(vm); // resolve injections before data/props
>      initState(vm);
>      initProvide(vm); // resolve provide after data/props
>      callHook(vm, 'created');
>
>      /* istanbul ignore if */
>      if (config.performance && mark) {
>        vm._name = formatComponentName(vm, false);
>        mark(endTag);
>        measure(("vue " + (vm._name) + " init"), startTag, endTag);
>      }
>
>      if (vm.$options.el) {
>        vm.$mount(vm.$options.el);
>      }
> ``` 
> <br>
## beforeCreate Hook
>  beforeCreate Hook은 lifecylce, event($on,$once,$off,$emit), render가 초기화 된후 실행되는 Hook이며 특징은 아래와 같다 
>  
> - vue 인스턴스가 초기화 된 직후에 발생
> - vue 인스턴스가 dom에 부착전(렌더링전) 및 data,methods,watch등이
vue instance에 injection 되기전에 실행되는 hook 이라 dom 접근 및 data,methods,watch 접근 불가능

## created Hook

>  created Hook은  vue instance에 property injection(data,props,watch,methods등) 후<br> data,props에 반응성 injection(data,props상태 변화 감시), provide injection 직후 호출되는 Hook 이며 특징은 아래와 같다
> - 해당 hook전에 data,methods,watch등이 instance injection되어 data등에 접근 가능
> -  data에 반응성이 injection된 상태
> -  vue instance가 dom에 부착전(렌더링전)이라 여전히 dom객체(element)에 접근 불가능
> - provide개념은 vue 2.2 부터 제공된 기능이며 부모-자식 컴포넌트 구조에 파이프라인을 만들어 주는 기능이다 
>   - 공식문서 참조 : https://kr.vuejs.org/v2/guide/components-edge-cases.html#%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85 


## 2. Mount
> 컴포넌트가 DOM에 추가될때 실행되는 라이프 사이클 Hook. 서버 사이드 렌더링을 지원 안함 컴포넌트가 DOM에 부탁된 이후라 DOM에 접근(this.$el)가능 하다 Mount에서 호출되는 hook 은 beforeMount,Mounted이다
> ```
> Vue.prototype.$mount = function (
>     el?: string | Element,
>     hydrating?: boolean
>   ): Component {
>     el = el && inBrowser ? query(el) : undefined
>     return mountComponent(this, el, hydrating)
>   }
> ```
>
> ```
>    Vue.prototype.$mount = function (
>      el?: string | Element,
>      hydrating?: boolean
>    ): Component {
>      el = el && inBrowser ? query(el) : undefined
>      // this => vue instance
>      // el => 옵션에 명시된 값이 => string일 경우   document.querySelector을 사용해서 dom 객체선택
>      return mountComponent(this, el, hydrating)
>    }
> ``` 
>```
>  function mountComponent (
>    vm,
>    el,
>    hydrating
>  ) {
>    vm.$el = el;
>    ...
>    callHook(vm, 'beforeMount');
>
>   var updateComponent;
>    /* istanbul ignore if */
>    
>    ....
>
>    // we set this to vm._watcher inside the watcher's constructor
>    // since the watcher's initial patch may call $forceUpdate (e.g. inside child
>    // component's mounted hook), which relies on vm._watcher being >already defined
>    new Watcher(vm, updateComponent, noop, {
>      before: function before () {
>        if (vm._isMounted && !vm._isDestroyed) {
>          callHook(vm, 'beforeUpdate');
>        }
>      }
>    }, true /* isRenderWatcher */);
>    hydrating = false;
>    // manually mounted instance, call mounted on self
>    // mounted is called for render-created child components in its inserted hook
>    if (vm.$vnode == null) {
>     vm._isMounted = true;
>    callHook(vm, 'mounted');
>    }
>    return vm
>  }
>```
> <br/>
> 

## beforeMount Hook

> 컴포넌트가 DOM 부착되기전에 실행 되는 Hook 
> - 가상 DOM이 생성되었으나 실제 DOM에 부착되지 않은 상태
> - beforeMount hook 이 실행된 후 mounted Hook이 실행되기전 미리 Watcher를 생성 해서 컴포넌트의 상태를 관찰한다

## mounted Hook
> 컴포넌트가 DOM에 부착되는 호출 되는 Hook 즉, 가상 DOM이 실제 DOM에 부착된 상태이다 그러므로 $el을 사용하여 DOM에 접근 할 수 있다.
> - mounted 훅이 호출되었다고 모든 컴포넌트가 mount 되었다고 보장할 수 없음.
> 모든 컴포넌트가 렌더링 보장된 상태에서 작업 하기 위해서 $nextTick 사용해야함
> - 자식 컴포넌트가 존재할 경우 자식 컴포넌트의 mounted hook이 부모 컴포넌트의 mounted hook 보다 먼저 실행 됨

## 3. Update
> 컴포넌트에서 사용되는 속성들이 변경되는 등의 컴포넌트가 재 랜더링 되면 실행되는 라이프 사이클 훅 컴포넌트가 재 렌더링 될 때, 변경 된 값으로 어떠한 작업을 해야 할 때 유용하게 사용 되는 훅임. 서버 사이드 렌더링은 지원하지 않음
해당 단계의 Hook 은 beforeUpdate,updated hook 이 있음
>```
>  function mountComponent (
>    vm,
>    el,
>    hydrating
>  ) {
>    vm.$el = el;
>    ...
>    callHook(vm, 'beforeMount');
>
>    var updateComponent;
>    /* istanbul ignore if */
>    if (config.performance && mark) {
>       ...
>    } else {
>      updateComponent = function () {
>        vm._update(vm._render(), hydrating);
>      };
>
>    // we set this to vm._watcher inside the watcher's constructor
>    // since the watcher's initial patch may call $forceUpdate (e.g. inside child
>    // component's mounted hook), which relies on vm._watcher being >already defined
>    new Watcher(vm, updateComponent, noop, {
>      before: function before () {
>        if (vm._isMounted && !vm._isDestroyed) {
>          callHook(vm, 'beforeUpdate');
>        }
>      }
>    }, true /* isRenderWatcher */);
>    hydrating = false;
>
>    if (vm.$vnode == null) {
>     vm._isMounted = true;
>    callHook(vm, 'mounted');
>    }
>    return vm
>  }
> ````
> ````
> updateComponent = function () {
>      // vm._update 은 lifecycleMixin 에서 정의    
>        vm._update(vm._render(), hydrating);
>  };
>   function lifecycleMixin (Vue) {
>    Vue.prototype._update = function (vnode, hydrating) {
>      var vm = this;
>      var prevEl = vm.$el;
>      var prevVnode = vm._vnode;
>      var restoreActiveInstance = setActiveInstance(vm);
>      vm._vnode = vnode;
>      // Vue.prototype.__patch__ is injected in entry points
>      // based on the rendering backend used.
>      if (!prevVnode) {
>        // initial render
>        vm.$el = vm.__patch__(vm.$el, vnode, hydrating, false /* removeOnly */);
>      } else {
>        // updates
>        vm.$el = vm.__patch__(prevVnode, vnode);
>      }
>      restoreActiveInstance();
>      // update __vue__ reference
>      if (prevEl) {
>        prevEl.__vue__ = null;
>      }
>      if (vm.$el) {
>        vm.$el.__vue__ = vm;
>      }
>      // if parent is an HOC, update its $el as well
>      if (vm.$vnode && vm.$parent && vm.$vnode === vm.$parent._vnode) {
>        vm.$parent.$el = vm.$el;
>      }
>      // updated hook is called by the scheduler to ensure that children are
>      // updated in a parent's updated hook.
>    };
> )
> 
> ````
> <br>

## beforeUpdate Hook
>  DOM이 재 렌더링 되기 직전에 호출되는 라이프 사이클 훅. 
> - 업데이트 된 값들을 가지고 있는 상태이기 때문에, 업데이트 된 값으로 다른 값들을 업데이트 할 수 있음
> - 재 렌더링 전의 새 상태의 데이터를 얻을 수 있고 더 많은 변경이 가능하다
> - 해당 Hook에서 값이 변경되더라도 다시 beforeUpdate Hook이 호출되지 않이 때문에 무한 루프에 빠지지 않음
> 
 
## updated 
> DOM이 재 렌더링 된 후 호출되는 라이프 사이클 훅 
> - DOM이 업데이트 된 후 호출 되는 훅이기 때문에 변경 된 후의 DOM을 이용해야 하는 처리를 할 때 사용하기 유용한 훅 
> - mounted 훅과 마찬가지로 재 렌더링이 끝났다는 것이 보장된 상태에서 작업하기 위해서는 $nextTick을 사용 
> - beforeUpdate 훅과 다르게 updated 훅에서 data를 수정하게 되면 update 훅이 호출 되기 때문에 무한 루프에 빠질 수 있으니 유의 

## 4.  Destroying
> 컴포넌트가 제거 될 때 실행되는 라이프 사이클 훅 
> ```
>  Vue.prototype.$destroy = function () {
>        const vm: Component = this
>        if (vm._isBeingDestroyed) {
>          return
>        }
>        callHook(vm, 'beforeDestroy')
>        vm._isBeingDestroyed = true
>        // remove self from parent
>        const parent = vm.$parent
>        if (parent && !parent._isBeingDestroyed && !vm.$options.abstract) {
>          remove(parent.$children, vm)
>        }
>        // teardown watchers
>        if (vm._watcher) {
>          vm._watcher.teardown()
>        }
>        let i = vm._watchers.length
>        while (i--) {
>          vm._watchers[i].teardown()
>        }
>        // remove reference from data ob
>        // frozen object may not have observer.
>        if (vm._data.__ob__) {
>          vm._data.__ob__.vmCount--
>        }
>        // call the last hook...
>        vm._isDestroyed = true
>        // invoke destroy hooks on current rendered tree
>        vm.__patch__(vm._vnode, null)
>        // fire destroyed hook
>        callHook(vm, 'destroyed')
>        // turn off all instance listeners.
>        vm.$off()
>        // remove __vue__ reference
>        if (vm.$el) {
>          vm.$el.__vue__ = null
>        }
>        // release circular reference (#6759)
>       if (vm.$vnode) {
>          vm.$vnode.parent = null
>        }
>      } 
> ```
> <br>
 
## beforeDestroy
> 컴포넌트가 제거 되기 직전에 호출되는 라이프 사이클 훅 
> - 컴포넌트는 본래의 기능들을 가지고 있는 온전한 상태 
> - 이 훅에서 이벤트 리스너를 해제하거나 컴포넌트에서 동작으로 할당 받은 자원들은 해제해야 할 때 사용하기 적합한 훅

## destroyed
> 컴포넌트가 제거 된 후 호출되는 라이프 사이클 훅
> - 컴포넌트의 모든 이벤트 리스너(@click, @change 등..)와 디렉티브(v-model, v-show 등..)의 바인딩이 해제 되고, 하위 컴포넌트도 모두 제거

 