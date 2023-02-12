3 ownership rules in Rust

1. Each value in Rust is owned by a variable
2. When the owner goes out of scope, the value is deallocated
3. There can only be **ONE** owner at a time

Borrowing
borrowing is down by reference 

```rust 
some_fn(&input)
some_fn2(&mut input)

fn some_fn(s: &String){

}

// or mutable ref
fn some_fn2(s: &mut String){
	s.push("asdf")
}
```

In the same scope, we can have as many references as we want, but only 1 mutable reference