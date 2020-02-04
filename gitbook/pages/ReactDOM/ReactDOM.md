# ReactDOM

## ReactDOM.render

### 参数

-   element : React\$Element
-   container : DOMContainer
-   callback : 回调函数

```js
    render(element,container,callback) {
        return legacyRenderSubtreeIntoContainer(
            null,
            element,
            container,
            false,
            callback
        );
    }
```

## legacyRenderSubtreeIntoContainer

-   parentComponent
-   children
-   container
-   forceHydrate
-   callback

```js
function legacyRenderSubtreeIntoContainer(
    parentComponent,
    children,
    container,
    forceHydrate,
    callback
) {
    invariant(
        isValidContainer(container),
        "Target container is not a DOM element."
    );

    let root = container._reactRootContainer;
    if (!root) {
        root = container._reactRootContainer = legacyCreateRootFromDOMContainer(
            container,
            forceHydrate
        );
        if (typeof callback === "function") {
            const originalCallback = callback;
            callback = function() {
                const instance = DOMRenderer.getPublicRootInstance(
                    root._internalRoot
                );
                originalCallback.call(instance);
            };
        }
        DOMRenderer.unbatchedUpdates(() => {
            if (parentComponent != null) {
                root.legacy_renderSubtreeIntoContainer(
                    parentComponent,
                    children,
                    callback
                );
            } else {
                root.render(children, callback);
            }
        });
    } else {
        if (typeof callback === "function") {
            const originalCallback = callback;
            callback = function() {
                const instance = DOMRenderer.getPublicRootInstance(
                    root._internalRoot
                );
                originalCallback.call(instance);
            };
        }
        // Update
        if (parentComponent != null) {
            root.legacy_renderSubtreeIntoContainer(
                parentComponent,
                children,
                callback
            );
        } else {
            root.render(children, callback);
        }
    }
    return DOMRenderer.getPublicRootInstance(root._internalRoot);
}
```

## legacyCreateRootFromDOMContainer

```js
function legacyCreateRootFromDOMContainer(container, forceHydrate) {
    const shouldHydrate =
        forceHydrate || shouldHydrateDueToLegacyHeuristic(container);
    if (!shouldHydrate) {
        let warned = false;
        let rootSibling;
        while ((rootSibling = container.lastChild)) {
            container.removeChild(rootSibling);
        }
    }

    const isConcurrent = false;
    return new ReactRoot(container, isConcurrent, shouldHydrate);
}
```

## ReactRoot

```js
function ReactRoot(container, isConcurrent, hydrate) {
    const root = DOMRenderer.createContainer(container, isConcurrent, hydrate);
    this._internalRoot = root;
}
```
