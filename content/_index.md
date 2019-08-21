---
title: "C++ and Rust"
outputs: "Reveal"
---

## The Symbiotic Relationship of C++ and Rust

<br>
<br>

<h4 class="right">Isabella Muerte</h4>
<h4 class="right">RustConf 2019</h4>

---

## Who am I?

{{% section %}}

 * {{%fragment%}}Hi, I'm Izzy Muerte{{%/fragment%}}  
   {{%fragment%}}<small class="smol">and you can too</small>{{%/fragment%}}

---

 * {{%fragment%}}C++ Bruja{{%/fragment%}}
 * {{%fragment%}}Aspirant Rustacean{{%/fragment%}}
 * {{%fragment%}}CMake War Criminal{{%/fragment%}}  
   {{%fragment%}}<small>(I promise not to talk about this today)</small>{{%/fragment%}}

---

 * {{%fragment%}}I care a lot about developer workflows{{%/fragment%}}
 * {{%fragment%}}I currently hold the record for most papers submitted to a single C++ standards meeting.{{%/fragment%}}{{%fragment%}}Completely by accident{{%/fragment%}}
 * {{%fragment%}}I started learning C++ about 10 years ago{{%/fragment%}}
 * {{%fragment%}}I've been paying attention to Rust since 0.6{{%/fragment%}}  
   {{%fragment%}}<small>(back when ~T and @T were still a thing and iterators were more ruby-like)</small>{{%/fragment%}}

---

 * <span class="fab fa-twitter">&nbsp;@slurpsmadrips</span>
 * <span class="fab fa-github">&nbsp;slurps-mad-rips</span>
 * {{%fragment%}}Octane Main in Apex Legends{{%/fragment%}}

{{%/section%}}

---

## Audience Boundaries

 * {{%fragment%}}Please leave all questions until after the talk is finished{{%/fragment%}}
 * {{%fragment%}}If we don't have time for questions, you can speak to me in person outside{{%/fragment%}}
 * {{%fragment%}}Lastly, you can always tweet a question with #123GottaCPP{{%/fragment%}}

---

## Further Resources

Extra talks for some additional context and understanding this talk's themes

 * {{%fragment%}}
   Elsewhere Memory (C++20 Abstract Machine)  
   &nbsp;&nbsp;&nbsp;&nbsp;&mdash; Niall Douglas, ACCU 2019
   {{%/fragment%}}
 * {{%fragment%}}
   Javascript Considered... Useful  
   &nbsp;&nbsp;&nbsp;&nbsp;&mdash; Jenn Schiffer, JSConf EU 2019
   {{%/fragment%}}
 * {{%fragment%}}
   The Tragedy of Systemd  
   &nbsp;&nbsp;&nbsp;&nbsp;&mdash; Benno Rice, BSDCan 2018 | linux.conf.au 2019
   {{%/fragment%}}

---

## Why are we here?

 * {{%fragment%}}C++ and Rust are going to continue living side by side for a long time to come{{%/fragment%}}
 * {{%fragment%}}Rust isn't going anywhere{{%/fragment%}}
 * {{%fragment%}}Neither is C++{{%/fragment%}}
 * {{%fragment%}}Might as well work together to learn from each other's mistakes{{%/fragment%}}

---

### Translating C++ and Rust

{{%section%}}

 * {{%fragment%}}Both languages share a lot of terms, but much like human languages we are drifting in terminology{{%/fragment%}}
 * {{%fragment%}}Both languages are trapped trying to express high level concepts at the machine code level. And no two machines are the same{{%/fragment%}}

---

|               Rust              |              C++             |
|:-------------------------------:|:----------------------------:|
|              `let`              |            `auto`            |
|              `::<>`             |   `::template` *or* `<>`     |
| `if let Some(x) = function() {` | `if (auto x = function()) {` |

---

|               Rust              |              C++             |
|:-------------------------------:|:----------------------------:|
|              `1i64`             |     `INT64_C(1)` (C++20)     |
|              `1i64`             |        `1i64` (C++23)        |
|           `1_000_000`           |          `1'000'000`         |

---

|               Rust              |              C++             |
|:-------------------------------:|:----------------------------:|
|         `impl Not for T`        |        `operator not`        |
|               move              |          relocation          |

---

Technically speaking, the following could be valid C++ or Rust code.

```rust
let x = Type::create("args")
  .whatever()
  .yourent()
  .my()
  .real()
  .compiler()
```

{{%fragment%}}Just `#define let auto` and you're on your way!{{%/fragment%}}

{{%/section%}}

---

## Differences and Compromises
