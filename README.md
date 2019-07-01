# Creating and Updating Key/Value Pairs

## Learning Goals

- Update hash values
- Add keys and values to a hash
- Find or create a hash value

## Introduction

While we're not able to read hash values using keys, currently, when we create
a hash, we're stuck with whatever data was entered in when the hash was created:

```ruby
person = {
  name: "Sam",
  age: 31
}
#=> {:name=>"Sam", :age=>31}

person[:age]
#=> 31
```

In this lesson, we're going to look at modifying and adding data to existing
hashes. This way, we can create a hash then update it as we need, letting us
maintain associations between pieces of data, even if the pieces themselves are
altered.

## Update Hash Values

Updating hash values is very similar to looking them up. For updating, we use
the **bracket-equals** method:

```ruby
person = {
  name: "Sam",
  age: 31
}
#=> {:name=>"Sam", :age=>31}

person[:age]
#=> 31

person[:age] = 32
#=> 32
```

If we look back at the entire hash, we see that the value associated with the
`:age` key has changed:

```ruby
person
#=> {:name=>"Sam", :age=>32}
```

Using the bracket-equals method, we can mutate any value stored inside a hash.
All we need to know is the associated key.

## Add Keys and Values to a Hash

In the previous lesson, we saw that using the bracket method and passing in an
invalid key returns `nil`:

```ruby
person[:hometown]
#=> nil
```

So, what happens when we use an invalid key with the bracket-equals method? When
Ruby discovers that the key is not present on the hash in question, Ruby will
simply _create_ a key/value pair on the hash:

```ruby
person = {
  name: "Sam",
  age: 31
}

# No :hometown key found
person[:hometown]
#=> nil

# Because :hometown was not present, Ruby creates the key value pair here
person[:hometown] = "Brooklyn, NY"
#=> "Brooklyn, NY"

# Now, the :hometown key refers to "Brooklyn, NY" when used in the brack method
person[:hometown]
#=> "Brooklyn, NY"

# Our original hash is also mutated
person
#=> {:name=>"Sam", :age=>31, :hometown=>"Brooklyn, NY"}
```

The general syntax for adding a new value to a hash is:
`hash[:new_key] = "New Value"`. `:new_key` is the literal new key we added to
the hash and we assigned the `:new_key` a value of `"New Value"`.

## Find or Create a Hash Value

We saw in the last lesson that the bracket method can be used in conditional
statements. One common use case of this is having to either find a value or 
_create_ that value. Let's consider what is involved.

First, let's take an example hash:

```ruby
shipping_manifest = {
  "whale bone corset" => 5,
  "porcelain vase" => 2,
  "oil painting" => 3,
  "silverware" => 34,
  "loom" => 1
}
```

Imagine the above hash is a manifest for products being shipped, with their
values representing quantity, and our job is to keep a tally as more products
are counted. A fourth oil painting shows up and we need to add it to the list.
Easy enough. The hash is small enough that we could just write the following and
be done:

```ruby
shipping_manifest["oil painting"] = 4
```

Three paintings previously accounted for, plus one new painting. However, we can
be a bit more abstract than that. Is there a way we can quickly increment an
integer without having to explicitly know the previous value? Well, we could do
this:

```ruby
shipping_manifest["oil painting"] = shipping_manifest["oil painting"] + 1
```

Now `shipping_manifest["oil painting"]` will be assigned to whatever it was
previously, _plus one_. If you recall from the looping lessons in Programming as
Conversation, there is an even shorter way to express this:

```ruby
shipping_manifest["oil painting"] += 1
```

Great! But wait.. what happens when a _new_ item is introduced. Say we need to
ship one top hat, which isn't present in the shipping_manifest yet.

```ruby
shipping_manifest["top hat"] += 1
```

If you plug the `shipping_manifest` hash into IRB and try the code snippet above,
you'll receive an error:

```text
NoMethodError (undefined method `+' for nil:NilClass)
```

What is happening here is that Ruby can't find `shipping_manifest["top hat"]`.
Because of this, it returns `nil`. As we know, Ruby doesn't like to combine data
types when it comes to operators. We are effectively writing `nil = nil + 1`,
which doesn't make any sense.

We can prevent this error from occurring by setting up a conditional and using
the bracket method to first look up a key before trying to change it:

```ruby
if shipping_manifest["top hat"]
  shipping_manifest["top hat"] += 1
else
  puts "Key not found!"
end
```

Since `"top hat"` isn't a key in `shipping_manifest`, the above conditional
will `puts` "Key not found!" to the terminal rather than cause an error.

This still doesn't fully solve the problem. Sure, we can't update something that
isn't there, but we still want to add a top hat to our shipping manifest.

Instead of just outputting a message to the terminal, we can handle adding
a key/value pair here.

```ruby
if shipping_manifest["top hat"]
  shipping_manifest["top hat"] += 1
else
  shipping_manifest["top hat"] = 1
end
```

Okay, so reading this in order - if `shipping_manifest["top hat"]` is truthy,
increment `shipping_manifest["top hat"]` by one. Else, assign
`shipping_manifest["top hat"]` to be equal to `1`.

Running the above conditional once again, the `"top hat"` key will
be added and set to `1`. Running it again will update `"top hat"` to `2`!

> **STRETCH**: If you think that this is a lot of code just to check and test for
> `nil`, we agree with you. There's a method on the Hash class that will let you
> look for a value and set a default if not found.

## Conclusion

With the ability to update values and create entirely new key/value pairs, we've
tackled the core concepts behind Ruby hashes. In the next few lessons, we will
reinforce these concepts with some lab practice.
