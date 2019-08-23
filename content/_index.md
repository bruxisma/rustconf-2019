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

 * <span class="fab fa-fw fa-twitter"></span>@slurpsmadrips
 * <span class="fab fa-fw fa-github"></span>slurps-mad-rips

{{%/section%}}

---

## Audience Boundaries

 * {{%fragment%}}Please leave all questions until after the talk is finished{{%/fragment%}}
 * {{%fragment%}}If we don't have time for questions, you can speak to me in person outside{{%/fragment%}}
 * {{%fragment%}}Lastly, you can always tweet a question with #123GottaCPP{{%/fragment%}}

---

## Further Resources

<p class="left">Extra talks for some additional context and understanding this talk's themes</p>

 * {{%fragment%}}Elsewhere Memory (C++20 Abstract Machine)  
   &nbsp;&nbsp;&nbsp;&nbsp;&mdash; Niall Douglas, ACCU 2019{{%/fragment%}}
 * {{%fragment%}}Javascript Considered... Useful  
   &nbsp;&nbsp;&nbsp;&nbsp;&mdash; Jenn Schiffer, JSConf EU 2019{{%/fragment%}}
 * {{%fragment%}}The Tragedy of Systemd  
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
|          Working Group          |          Study Group         |

---

Technically speaking, the following could be valid C++ or Rust code.

```rust
let x = Type::create("args")
  .whatever()
  .yourent()
  .my()
  .real()
  .space()
  .compiler()
```

{{%fragment%}}Just `#define let auto` and you're on your way!{{%/fragment%}}

{{%/section%}}

---

## Differences

{{%section%}}

 * {{%fragment%}}Let's not talk about defaults.{{%/fragment%}}
 * {{%fragment%}}More of a dead meme than RIIR{{%/fragment%}}

---

Instead let's talk about

 * {{%fragment%}}concepts vs traits{{%/fragment%}}
 * {{%fragment%}}execution context boundary{{%/fragment%}}

{{%/section%}}

---

## Traits vs. Concepts

{{%section%}}

{{%fragment%}}Rust Traits are *maximally* constraining.{{%/fragment%}}{{%fragment%}}If some `T` does not mention what traits it uses, it cannot be called on anything{{%/fragment%}}
{{%fragment%}}C++ Concepts are *minimally* constraining{{%/fragment%}}{{%fragment%}}A given `T` must at least meet the requirements of a set of concepts.{{%/fragment%}}{{%fragment%}}Additionally special members are "best fit" as of Cologne 2019{{%/fragment%}}

---

```cpp
template <std::destructible T>
struct optional {
  constexpr ~optional ()
    requires std::trivially_destructible<T> = default 
  ~optional()
    requires negate<std::trivially_destructible<T>>
    noexcept { this->reset(); }
};
```

{{%/section%}}

---

## Execution Context Boundary

{{%section%}}

{{%fragment%}}AKA "Executing code at compile time vs runtime"{{%/fragment%}}  
{{%fragment%}}const generics vs constexpr/consteval{{%/fragment%}}
{{%fragment%}}C++ has a lot more work in this area{{%/fragment%}}

---

{{%fragment%}}C++20: `consteval`, `constinit`, and `constexpr`{{%/fragment%}}

 * {{%fragment%}}We're not going to talk about `constinit`{{%/fragment%}}
 * {{%fragment%}}`consteval`: Only at compile time{{%/fragment%}}
 * {{%fragment%}}`contexpr`: Sometimes at compile time{{%/fragment%}}{{%fragment%}}(effectively, a bridge){{%/fragment%}}
 * {{%fragment%}}functions are runtime by default{{%/fragment%}}

---

 * {{%fragment%}}We now support `virtual constexpr`{{%/fragment%}}{{%fragment%}}i.e., `virtual constexpr ~T ();`{{%/fragment%}}

---

 * {{%fragment%}}If we can do it, Rust can do it{{%/fragment%}}

{{%/section%}}

---

## Standards vs Specifications

{{%section%}}

 * All standards are a specification
 * Not all specifications are a standard, especially if they apply to a single
   product.
 * Thus, there could be a "standard" for Rust in the future which borrowck is
   not a part of. Yes, this is terrifying, but let's put that aside for now.

---

 * C++ is limited to the rules of ISO, but we have a standard.
 * Rust is limited to the the laws of the United States under which Mozilla operates.

---

 * ISO draws its authority from multiple nations, each backed by treaties,
   international trademark laws, (in some cases, militaries) and by owning the
   content found in its documents. (i.e., the "standard plate of spahgetti" is
   owned by ISO)
 * ECMA, an alternative organization to ISO, draws its authority from the same
   sources. However corporations have as much of a say as a national body.
 * Mozilla draws its authority from United States corporate laws. Such as
   intellectual property laws that can be used to threaten permanent desitution
   to anyone violating them if a court deems that someone has violated these
   laws.

---

 * I'm not suggesting

{{%/section%}}

---

## Who do you want?


