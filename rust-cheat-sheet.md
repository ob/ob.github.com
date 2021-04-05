---
layout: page
title:  "Rust cheat sheet"
date:   2017-08-28 21:28:49 -0700
categories: rust
---

This is just a quick and dirty rust cheat sheet.

* auto-gen TOC:
{:toc}

# Printing

When printing using `println!()`:

* `{}` uses the `Display` Trait
* `{:?}` uses the `Debug` Trait
* `{:#?}` uses the `Debug` Trait but prints the output in multiple lines.

# Iterators

* `.iter()` immutable references.
* `.into_iter()` owned values.
* `.iter_mut()` mutable references.

# Command Line Arguments

[Clap](https://github.com/kbknapp/clap-rs) is the way to go.

{% highlight toml %}
[dependencies]
clap = "2"
{% endhighlight %}

Then look at the
[examples](https://github.com/kbknapp/clap-rs/tree/master/examples) or
read the [documentation](https://docs.rs/clap/2.26.0/clap/).

If you're looking for a quick & dirty usage, try something like this:

{% highlight rust %}
extern crate clap;
use clap::{App, Arg};

fn main() {
  let matches = App::new("app")
	.version("v1.0-beta")
	.arg(Arg::with_name("argname")
		.help("The help goes here")
		.index(1) // Positional argument
		.required(true)
	)
	.get_matches();
	
  let argname = matches.value_of("argname").unwrap(); // Safe because required
  println!("The argname is {}", argname);
}
{% endhighlight %}

# Reading a file line by line

{% highlight rust %}
use std::io;
use std::io::{BufReader, BufRead};
use std::fs::File;

fn readFile(filename: &str) -> Result<_, io::Error> {
	let f = File::open(filename)?;
	let r = BufReader::new(&f);
	for line in r.lines() {
		let l = line.unwrap();
		println!("The line is: {}", l);
	}
}

{% endhighlight %}

# Errors

This is mostly for `io::Error`

{% highlight rust %}
use std::io::{Error, ErrorKind};

// errors can be created from strings
let custom_error = Error::new(ErrorKind::Other, "oh no!");

// errors can also be created from other errors
let custom_error2 = Error::new(ErrorKind::Interrupted, custom_error);
{% endhighlight %}

