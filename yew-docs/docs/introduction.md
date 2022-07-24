`html! {}` 宏可以让我们在 `Rust`中写 html，如下例子：

```rust
use yew::html;

html! {
    <div>
        <div data-key="abc"></div>
        <div class="parent">
            <span class="child" value="anything"></span>
            <label for="first-name">{ "First Name" }</label>
            <input type="text" id="first-name" value="placeholder" />
            <input type="checkbox" checked=true />
            <textarea value="write a story" />
            <select name="status">
                <option selected=true disabled=false value="">{ "Selected" }</option>
                <option selected=false disabled=true value="">{ "Unselected" }</option>
            </select>
        </div>
    </div>
};
```

条件渲染

```rust
use yew::html;

html! {
    if true {
        <p>{ "True case" }</p>
    }
};
```

`if let else`

```rust
use yew::html;
let some_text = Some("text");

html! {
    if let Some(text) = some_text {
        <p>{ text }</p>
    }
};
```

### 循环输出 html

```rust
use yew::prelude::*;


#[function_component(App)]
pub fn app() -> Html {

    let title_enable = false;

    let lists = vec![
        "item1", "item2", "item3", "item4", "item5", "item6", "item7", "item8",
    ];
    html! {
        <>
            if title_enable {
                <h1 class="title">{"Hello Yew"}</h1>
            } else {
                <h1 class="title">{"无标题"}</h1>
            }

            <ul>{list_to_html(lists)}</ul>
        </>
    }
}

fn list_to_html(list: Vec<&str>) -> Vec<Html> {
    list.iter()
        .map(|item| {
            html! {
                <li>{item}</li>
            }
        })
        .collect()
}

```
