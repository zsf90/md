### 内部样式

第一个例子

```rust
use gloo::console::log;

use stylist::{style, yew::styled_component};
use yew::prelude::*;

#[styled_component(App)]
pub fn app() -> Html {
    let stylesheet = style!(
        r#"
            h1 {
                color: orange;
            }

            p {
                font-size: 14px;
                color: #00cccc;
            }

            ul {
                list-style: none;
                display: inline-block;
            }

            ul li {
                display: inline-block;
                padding: 4px;
            }

        "#
    )
    .unwrap();
    let title_enable = true;

    let lists = vec![
        "item1", "item2", "item3", "item4", "item5", "item6", "item7", "item8",
    ];
    html! {
        <div class={stylesheet}>
            if title_enable {
                <h1>{"Hello Yew"}</h1>
            } else {
                <h1 class="title">{"无标题"}</h1>
            }

            <ul>{list_to_html(lists)}</ul>

            <p>{"fetching cargo artifacts"}</p>
        </div>
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

### 行内样式

例子

```rust
use gloo::console::log;

use stylist::{css, style, yew::styled_component};
use yew::prelude::*;

#[styled_component(App)]
pub fn app() -> Html {
    let stylesheet = style!(
        r#"
            h1 {
                color: orange;
            }

            p {
                font-size: 14px;
                color: #00cccc;
            }

            ul {
                list-style: none;
                display: inline-block;
            }

            ul li {
                display: inline-block;
                padding: 4px;
            }

        "#
    )
    .unwrap();
    let title_enable = true;

    let lists = vec![
        "item1", "item2", "item3", "item4", "item5", "item6", "item7", "item8",
    ];
    html! {
        <div class={stylesheet}>
            if title_enable {
                <h1>{"Hello Yew"}</h1>
            } else {
                <h1 class="title">{"无标题"}</h1>
            }

            <ul>{list_to_html(lists)}</ul>

            <p>{"fetching cargo artifacts"}</p>
        </div>
    }
}

fn list_to_html(list: Vec<&str>) -> Vec<Html> {
    list.iter()
        .map(|item| {
            html! {
                <li class={css!(color: black; font-weight: bold;)}>{item}</li>
            }
        })
        .collect()
}
```

## 外部 css 文件

```rust
use gloo::console::log;

use stylist::{css, style, yew::styled_component, Style};
use yew::prelude::*;

const STYLE_FILE: &str = include_str!("../../style.css");

#[styled_component(App)]
pub fn app() -> Html {
    let stylesheet = Style::new(STYLE_FILE).unwrap();
    let title_enable = true;

    let lists = vec![
        "item1", "item2", "item3", "item4", "item5", "item6", "item7", "item8",
    ];
    html! {
        <div class={stylesheet}>
            if title_enable {
                <h1>{"Hello Yew"}</h1>
            } else {
                <h1 class="title">{"无标题"}</h1>
            }

            <ul>{list_to_html(lists)}</ul>

            <p>{"fetching cargo artifacts"}</p>
        </div>
    }
}

fn list_to_html(list: Vec<&str>) -> Vec<Html> {
    list.iter()
        .map(|item| {
            html! {
                <li class={css!(color: black; font-weight: bold;)}>{item}</li>
            }
        })
        .collect()
}

```

带参数的组件

```rust
use yew::prelude::*;

#[derive(Properties, PartialEq)]
pub struct Props {
    pub title: String,
}

#[function_component(MainTitle)]
pub fn main_title(props: &Props) -> Html {
    html! {<h1>{&props.title}</h1>}
}
```

```rust
use gloo::console::log;

use crate::components::atoms::main_title::MainTitle;
use stylist::{css, style, yew::styled_component, Style};
use yew::prelude::*;

const STYLE_FILE: &str = include_str!("../../style.css");

#[styled_component(App)]
pub fn app() -> Html {
    let stylesheet = Style::new(STYLE_FILE).unwrap();
    let title_enable = true;

    let lists = vec![
        "item1", "item2", "item3", "item4", "item5", "item6", "item7", "item8",
    ];
    html! {
        <div class={stylesheet}>
            if title_enable {
                <MainTitle title="hello RUST!" />
                <MainTitle title="hello YEW!" />
            } else {
                <h1 class="title">{"无标题"}</h1>
            }

            <ul>{list_to_html(lists)}</ul>

            <p>{"fetching cargo artifacts"}</p>
        </div>
    }
}

fn list_to_html(list: Vec<&str>) -> Vec<Html> {
    list.iter()
        .map(|item| {
            html! {
                <li class={css!(color: black; font-weight: bold;)}>{item}</li>
            }
        })
        .collect()
}

```

### 组件间传递“枚举”

```rust
use gloo::console::log;

use crate::components::atoms::main_title::{Color, MainTitle};
use stylist::{css, yew::styled_component};
use yew::prelude::*;

#[styled_component(App)]
pub fn app() -> Html {
    let title_enable = true;

    let lists = vec![
        "item1", "item2", "item3", "item4", "item5", "item6", "item7", "item8",
    ];
    html! {
        <div>
            if title_enable {
                <MainTitle title="hello RUST!" color={Color::Error}/>
                <MainTitle title="hello YEW!" color={Color::Ok}/>
            } else {
                <h1 class="title">{"无标题"}</h1>
            }

            <ul>{list_to_html(lists)}</ul>

            <p>{"fetching cargo artifacts"}</p>
        </div>
    }
}

fn list_to_html(list: Vec<&str>) -> Vec<Html> {
    list.iter()
        .map(|item| {
            html! {
                <li class={css!(color: black; font-weight: bold;)}>{item}</li>
            }
        })
        .collect()
}

```

```rust
use stylist::{css, style, yew::styled_component, Style};
use yew::prelude::*;

#[derive(Properties, PartialEq)]
pub struct Props {
    pub title: String,
    pub color: Color,
}

#[derive(PartialEq)]
pub enum Color {
    Normal,
    Ok,
    Error,
}

impl Color {
    pub fn to_string(&self) -> String {
        match self {
            Color::Normal => "normal".to_owned(),
            Color::Ok => "ok".to_owned(),
            Color::Error => "error".to_owned(),
        }
    }
}

#[function_component(MainTitle)]
pub fn main_title(props: &Props) -> Html {
    let stylesheet = style!(
        r#"
            .normal {
                color: white;
            }

            .ok {
                color: green;
            }

            .error {
                color: red;
            }
        "#
    )
    .unwrap();
    html! {
        <div class={stylesheet}>
            <h1 class={props.color.to_string()}>{&props.title}</h1>
        </div>
    }
}

```

### Callback

```rust
use stylist::{css, style, yew::styled_component, Style};
use yew::prelude::*;

#[derive(Properties, PartialEq)]
pub struct Props {
    pub title: String,
    pub color: Color,
    pub on_load: Callback<String>,
}

#[derive(PartialEq)]
pub enum Color {
    Normal,
    Ok,
    Error,
}

impl Color {
    pub fn to_string(&self) -> String {
        match self {
            Color::Normal => "normal".to_owned(),
            Color::Ok => "ok".to_owned(),
            Color::Error => "error".to_owned(),
        }
    }
}

#[function_component(MainTitle)]
pub fn main_title(props: &Props) -> Html {
    let stylesheet = style!(
        r#"
            .normal {
                color: white;
            }

            .ok {
                color: green;
            }

            .error {
                color: red;
            }
        "#
    )
    .unwrap();

    props.on_load.emit("Im loaded!!!!".to_owned());

    html! {
        <div class={stylesheet}>
            <h1 class={props.color.to_string()}>{&props.title}</h1>
        </div>
    }
}
```

```rust
use gloo::console::log;
use yew::prelude::*;

use crate::components::atoms::main_title::{Color, MainTitle};

#[function_component(App)]
pub fn app() -> Html {
    let main_title_load = Callback::from(|message: String| log!(message));
    html! {
        <div>
            <MainTitle title={"Hello,World!"} color={Color::Ok} on_load={main_title_load}/>
        </div>
    }
}
```
