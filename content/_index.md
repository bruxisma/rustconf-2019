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
   {{%fragment%}}
   {{<tweet 1163933491471560704 >}}
   {{%/fragment%}}

---

 * {{%fragment%}}I care a lot about developer workflows{{%/fragment%}}
 * {{%fragment%}}I currently hold the record for most papers submitted to a single C++ standards meeting.{{%/fragment%}}{{%fragment%}}Completely by accident{{%/fragment%}}
 * {{%fragment%}}I started learning C++ about 10 years ago{{%/fragment%}}
 * {{%fragment%}}I've been paying attention to Rust for a while{{%/fragment%}}  
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

---

Because C++'s memory model is used for hardware (such as NVidia's CUDA),
Rust will also have to at least meet the same memory model at some level.

 * {{%fragment%}}See Niall's talk about the Abstract Machine for more information{{%/fragment%}}

{{%/section%}}

---

## Differences

{{%section%}}

 * {{%fragment%}}Let's not talk about defaults.{{%/fragment%}}
 * {{%fragment%}}More of a dead meme than RIIR{{%/fragment%}}

---

Instead let's talk about

 * {{%fragment%}}Lifetime Semantics{{%/fragment%}}
 * {{%fragment%}}concepts vs traits{{%/fragment%}}
 * {{%fragment%}}execution context boundary{{%/fragment%}}{{%fragment%}}(We don't really have a term for this...){{%/fragment%}}

{{%/section%}}

---

## Rust Lifetime Semantics

 * {{%fragment%}}Baked into the language{{%/fragment%}}
 * {{%fragment%}}Most of the time mappable to existing APIs{{%/fragment%}}
 * {{%fragment%}}Custom semantics can break down (ObjC, OpenCL){{%/fragment%}}

---

## C++ Lifetime Semantics

 * {{%fragment%}}LOL, what semantics???{{%/fragment%}}
 * {{%fragment%}}Library defined{{%/fragment%}}{{%fragment%}}(Mostly...){{%/fragment%}}

---

## DRES

 * AKA "Destructors Run at Exit Scope"

---

## For example

 * {{%fragment%}}`retain_ptr`{{%/fragment%}}  

{{%fragment%}}Works with:{{%/fragment%}}

 * {{%fragment%}}C APIs{{%/fragment%}}
 * {{%fragment%}}1990s designed OOP types (COM, DirectX){{%/fragment%}}
 * {{%fragment%}}Web Browsers{{%/fragment%}}
 * {{%fragment%}}Async APIs{{%/fragment%}}
 * {{%fragment%}}Introduces adoption as a lifetime semantic{{%/fragment%}}

---

## Traits vs. Concepts

{{%section%}}

 * {{%fragment%}}Rust Traits are *maximally* constraining.{{%/fragment%}}
 * {{%fragment%}}If some `T` does not mention what traits it uses, it cannot be called on anything{{%/fragment%}}

---

 * {{%fragment%}}C++ Concepts are *minimally* constraining{{%/fragment%}}
 * {{%fragment%}}A given `T` must at least meet the requirements of a set of concepts.{{%/fragment%}}
 * {{%fragment%}}Additionally special members are "best fit" as of Cologne 2019{{%/fragment%}}

---

```cpp
template <std::destructible T>
struct optional {
  constexpr ~optional ()
    requires std::trivially_destructible<T> = default 
  ~optional()
    requires std::negate<std::trivially_destructible<T>>
    noexcept { this->reset(); }
};
```

{{%/section%}}

---

## Building With Concepts

{{%section%}}

You know what traits look like in practice, so let's look at using a concept.

---

```cpp
template <class T>
concept HasElementType = requires (T object) { typename T::element_type; };
template <class T>
concept HasPointer = requires (T object) { typename T::pointer; };

template <class T> requires HasElementType<T> and not HasPointer<T>
std::add_pointer_t<typename T::element_type> pointer_type_of () noexcept;

template <class T> requires HasPointer<T>
typename T::pointer pointer_type_of () noexcept;

template <class T> using pointer_type_of_t = decltype(pointer_type_of<T>());
```

---

(We did change to `snake_case` concepts recently, but I didn't feel like updating all the code)

```cpp
template <class T>
concept SlightlyRegular =
  std::StrictTotallyOrderedWith<T, std::nullptr_t> and
  std::EqualityComparableWith<T, std::nullptr_t> and
  std::StrictTotallyOrdered<T> and
  std::EqualityComparable<T> and
  std::Constructible<T, impl::pointer_type_of_t<T>> and
  std::Movable<T>;
```

---

```cpp
template <class T>
concept PointerLike = requires (T object) {
  requires SlightlyRegular<T>;

  { object.get() } noexcept -> impl::pointer_type_of_t<T>;
  { static_cast<bool>(object) } noexcept;
  { object.reset() } noexcept;
  { object.reset(std::declval<pointer_type_of_t<T>>()) } noexcept;
  { object.operator->(); } noexcept -> impl::pointer_type_of_t<T>;
  { *object } noexcept -> decltype(*object.get());
};
```

---

```cpp
template <class T, PointerLike Storage>
struct handle {
  using storage_type = Storage;
  using pointer = impl::pointer_type_of_t<storage_type>;

  explicit operator bool () const noexcept { return bool(this->storage); }
  decltype(auto) get () const noexcept { return this->storage.get(); }
  void reset (pointer ptr = pointer { }) noexcept {
    this->storage.reset(ptr);
  }

protected:
  handle (pointer ptr) noexcept : storage { ptr } { }
  storage_type storage;
};
```

---

```cpp
template <class T, class D=std::default_delete<T>>
using unique_handle = handle<T, std::unique_ptr<T, D>>;

template <class T>
using shared_handle = handle<T, std::shared_ptr<T>>;

template <class T, class R=std::retain_traits<T>>
using retain_handle = handle<T, std::retain_ptr<T, R>>;

template <class T>
using viewed_handle = handle<T, std::experimental::observer_ptr<T>>;
```

---

```cpp
// This uses the API from the android NDK
#include <configuration.h>

namespace android::cfg {

enum class orientation {
  any = ACONFIGURATION_ORIENTATION_ANY,
  portrait = ACONFIGURATION_ORIENTATION_PORT,
  landscape = ACONFIGURATION_ORIENTATION_LAND,
  square = ACONFIGURATION_ORIENTATION_SQUARE
};

} /* namespace android::cfg */
```
---

```cpp
namespace android {
struct deleter { void operator (AConfiguration*) const noexcept; };

struct configuration : private unique_handle<AConfiguration, deleter> {
  configuration (configuration const& that) noexcept : configuration { }
  { AConfiguration_copy(std::out_ptr(this), that.get()) }

  configuration () noexcept : unique_handle(AConfiguration_new()) { }

  cfg::orientation orientation () const noexcept {
    return cfg::orientation(AConfiguration_getOrientation(this->get()));    
  }
};
} /* namespace android */
```

---

{{%fragment%}}In just seven slides we described a way to allow per-type opt-in
lifetime semantics.{{%/fragment%}}

* {{%fragment%}}This can be extended later by implementing other `PointerLike` types{{%/fragment%}}

---

 * {{%fragment%}}`hazard_ptr`{{%/fragment%}}
 * {{%fragment%}}`boost::offset_ptr`{{%/fragment%}}
 * {{%fragment%}}`go_ptr`{{%/fragment%}}

{{%/section%}}

---

## Execution Context Boundary

{{%section%}}

 * {{%fragment%}}AKA "Executing code at compile time vs runtime"{{%/fragment%}}  
 * {{%fragment%}}const generics vs `constexpr`/`consteval`{{%/fragment%}}
 * {{%fragment%}}C++ has explored a lot more work in this area{{%/fragment%}}

---

{{%fragment%}}C++20: `consteval`, `constinit`, and `constexpr`{{%/fragment%}}

 * {{%fragment%}}We're not going to talk about `constinit`{{%/fragment%}}
 * {{%fragment%}}functions are runtime by default{{%/fragment%}}
 * {{%fragment%}}`consteval`: Only at compile time{{%/fragment%}}
 * {{%fragment%}}`contexpr`: Sometimes at compile time{{%/fragment%}}{{%fragment%}}(effectively, a bridge){{%/fragment%}}

---

 * {{%fragment%}}We now support `virtual constexpr`{{%/fragment%}}  
 {{%fragment%}}i.e., `virtual constexpr ~T ();`{{%/fragment%}}

---

 * {{%fragment%}}If we can do it, Rust can do it{{%/fragment%}}

{{%/section%}}

---

## Who do you want?

{{%section%}}

In Rice's talk he mentions that the FreeBSD community was telling those unhappy
with SystemD to come to FreeBSD.

{{%fragment%}}These are however, not the people you want in your community{{%/fragment%}}

---

Likewise, telling people dissatisfied with C++'s design structure that they should come to Rust is not something you want.


{{%fragment%}}
{{< tweet 1156281666127556608 >}}
{{%/fragment%}}

---

I'd like to show a few other tweets from the VP of Technology at Activision

{{%fragment%}}But he blocked me on twitter when I said a game company making billions could easily afford to send representatives, and paying their employees a decent salary{{%/fragment%}}

{{%fragment%}}`¯\\\_(ツ)_/¯`{{%/fragment%}}

---

I don't think you want these people in the Rust milieu

{{%fragment%}}But if we start to see other implementations arise you may not have a choice about interacting with them.{{%/fragment%}}

{{%/section%}}

---

## Standards vs Specifications

{{%section%}}

 * {{%fragment%}}All standards are a specification{{%/fragment%}}
 * {{%fragment%}}Not all specifications are a standard, especially if they
   apply to a single product. {{%/fragment%}}

 * {{%fragment%}}Thus, there could be a "standard" for Rust in the future which
   borrowck is not a part of.{{%/fragment%}}
   {{%fragment%}}Yes, this is terrifying, but let's put that aside for now.{{%/fragment%}}

---

 * C++ is limited to the rules of ISO, but we have a standard.
 * {{%fragment%}}Rust is limited to the the laws of the United States under which Mozilla operates.{{%/fragment%}}

---


 * ISO draws its authority from multiple nations, each backed by treaties,
   international trademark laws, (in some cases, militaries) and by owning the
   content found in its documents. (i.e., the "standard plate of spahgetti" is
   owned by ISO)
 * {{%fragment%}}Membership access changes per country, but some money is always required.{{%/fragment%}}

---

 * ECMA, an alternative organization to ISO, draws its authority from the same
   sources. However corporations have as much of a say as a national body
 * {{%fragment%}}Voting rights are prohibitively expensive{{%/fragment%}}

---

 * Mozilla draws its authority from United States corporate laws. Such as
   intellectual property laws that can be used to threaten permanent desitution
   to anyone violating them if a court deems that someone has violated these
   laws.

---

 * In the above cases, no one has a real say.
 * {{%fragment%}}Without some labor derived reason (i.e., work) changes to C++
   or Rust might be dismissed.{{%/fragment%}}

 * {{%fragment%}}This takes power away from users, and replaces it with submission to authority{{%/fragment%}}

---

 * Spoken and written languages like English exist to express our thoughts and desires and communicate this to others. Programming languages are the same, otherwise we wouldn't use the word "language" to describe them.
 * {{%fragment%}}Forcibly changing a language instead of letting it evolve naturally is foolish{{%/fragment%}}

---

 * C++'s charter is to standardize existing practice and evolve the language to make expression for users easier.
 * {{%fragment%}}In practice, corporations tend to get their way so they have to maintain less code{{%/fragment%}}
 * {{%fragment%}}As an example, `retain_ptr`'s defaults{{%/fragment%}}

---

 * I'm not suggesting Rust be standardized in the above structures.
 {{%fragment%}}But something will be needed. RustC is not going to be the only
 implementation one day.{{%/fragment%}}

 * {{%fragment%}}A specification for the language doesn't mean anything if I can
   say "I follow 80% of the Rust specification, I've documented divergence"{{%/fragment%}}

---

 * Having a document say "You cannot say you implement part of this
   specification" doesn't mean that the tools surrounding said implementation
   have to behave well.
 * {{%fragment%}}Cargo is not something that can be standardized, but
   its behavior and interface is something that could be specified.{{%/fragment%}}

---

 * Jenn's talk about JS briefly mentions that she would like to see Javascript
 evolve without something that closes off its voting process.
 * {{%fragment%}}Likewise, there are several people on WG21 that want to see C++ evolve without ISO's limitations{{%/fragment%}}
 * {{%fragment%}}I would not be surprised if people in this room would agree{{%/fragment%}}

---

If Microsoft or Facebook announced tomorrow that they were implementing a rust
compiler so they could `extern "Rust"` from C++, you would actively see real
time divergence.

{{%fragment%}}They can say "We are *like* rust" and not violate trademark laws.{{%/fragment%}}

{{%fragment%}}Mozilla's authority is undermined and power is taken away from the Core team{{%/fragment%}}

--- 

Without exploration of alternatives to standardization, the current open process of development can be closed off.

{{%fragment%}} Authorityis placed into the hands of other corporate or national body representatives{{%/fragment%}}

{{%fragment%}} And without the consent of those most passionate about Rust{{%/fragment%}}

---

Arguments are made that this is to reduce the signal to noise ratio

{{%fragment%}}In reality it shows apathy and unwillingness to focus on communication{{%/fragment%}}

---

So what's needed?

{{%fragment%}}I'm an anarchist{{%/fragment%}}

{{%fragment%}}I don't pretend like I have all the answers{{%/fragment%}}

{{%fragment%}}I can't promise everything is going to be ok{{%/fragment%}}

{{%fragment%}}I can't guarantee you're going to win{{%/fragment%}}

---

A collective, a forum?

{{%fragment%}}An *informal* body for a *formal* standard does not exist.{{%/fragment%}}{{%fragment%}}...yet{{%/fragment%}}

{{%fragment%}}I don't even know if its possible{{%/fragment%}}

{{%fragment%}}Royalty Free standards exist, but still require paid membership{{%/fragment%}}

---

You all have a responsibility to yourselves, and your communities, but also
to the industries we work in at large.

---

{{%/section%}}

---

{{< slide background-image="evangelion.jpg" >}}

{{< tweet 1156972708778803200 >}}

