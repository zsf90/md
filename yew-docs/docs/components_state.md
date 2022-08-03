# 组件

## custom_form.rs

```rust
use crate::components::atoms::{custom_button::CustomButton, text_input::TextInput};
use std::ops::Deref;
use yew::prelude::*;

#[derive(Default, Clone)]
struct Data {
    pub username: String,
    pub count: u32,
}

#[function_component(CustomForm)]
pub fn custom_form() -> Html {
    let state = use_state(|| Data::default());
    let cloned_state = state.clone();
    let username_changed = Callback::from(move |username| {
        // let mut data = cloned_state.deref().clone();
        // data.username = username;
        // cloned_state.set(data);
        cloned_state.set(Data {
            username,
            ..cloned_state.deref().clone()
        });
    });

    let cloned_state = state.clone();
    let button_clicked = Callback::from(move |_| {
        let mut data = cloned_state.deref().clone();
        data.count += 1;
        cloned_state.set(data);
    });

    html! {
        <div>
            <TextInput name="username" handle_onchange={username_changed} />
            <CustomButton label="Submit" onclick={button_clicked}/>
            <p>{&state.username}</p>
            <p>{"Button has been clicked "} {state.count} {" times"}</p>
        </div>
    }
}

```
