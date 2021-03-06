# Rofi Library for Rust
Spawn [rofi](https://github.com/davatorium/rofi) windows, and parse the result appropriately.

## Simple example

```
use rofi;
use std::{fs, env};

let dir_entries = fs::read_dir(env::current_dir().unwrap())
    .unwrap()
    .map(|d| format!("{:?}", d.unwrap().path()))
    .collect::<Vec<String>>();

match rofi::Rofi::new(&dir_entries).run() {
    Ok(choice) => println!("Choice: {}", choice),
    Err(rofi::Error::Interrupted) => println!("Interrupted"),
    Err(e) => println!("Error: {}", e)
}
```

## Example of returning an index
`rofi` can also be used to return an index of the selected item:

```
use rofi;
use std::{fs, env};

let dir_entries = fs::read_dir(env::current_dir().unwrap())
    .unwrap()
    .map(|d| format!("{:?}", d.unwrap().path()))
    .collect::<Vec<String>>();

match rofi::Rofi::new(&dir_entries).run_index() {
    Ok(element) => println!("Choice: {}", element),
    Err(rofi::Error::Interrupted) => println!("Interrupted"),
    Err(rofi::Error::NotFound) => println!("User input was not found"),
    Err(e) => println!("Error: {}", e)
}
```

## Example of using pango formatted strings
`rofi` can display pango format. Here is a simple example (you have to call
the `self..pango` function).

```
use rofi;
use rofi::pango::{Pango, FontSize};
use std::{fs, env};

let entries: Vec<String> = vec![
    Pango::new("Option 1").size(FontSize::Small).fg_color("#666000").build(),
    Pango::new("Option 2").size(FontSize::Large).fg_color("#deadbe").build(),
];

match rofi::Rofi::new(&entries).pango().run() {
    Ok(element) => println!("Choice: {}", element),
    Err(rofi::Error::Interrupted) => println!("Interrupted"),
    Err(e) => println!("Error: {}", e)
}
```
