# Let's Talk About Functional Programming

# Complexity is the Enemy

## The simpler that we make our code:
- the simpler it is to understand
- the simpler it is to change
- the simpler it is to test
- the simpler it is to ship

# The Object Oriented Model

- Objects are a combination of data and methods do stuff related to that data
- Objects are often mutable, and methods often mutate objects as side effects
- “Object-oriented programming is an exceptionally bad idea which could only have originated in California.” – Edsger Dijkstra
- “The phrase "object-oriented” means a lot of things. Half are obvious, and the other half are mistakes.“ – Paul Graham
- “Implementation inheritance causes the same intertwining and brittleness that have been observed when goto statements are overused. As a result, OO systems often suffer from complexity and lack of reuse.” – John Ousterhout Scripting, IEEE Computer, March 1998
- “Sometimes, the elegant implementation is just a function. Not a method. Not a class. Not a framework. Just a function.” – John Carmack
- “The problem with object-oriented languages is they’ve got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana and the entire jungle.” – Joe Armstrong
- “I used to be enamored of object-oriented programming. I’m now finding myself leaning toward believing that it is a plot designed to destroy joy.” – Eric Allman

# The Mathematical Model

- There are no side effects in math
- There is no http, ssd, ram, or display
- Numbers are not mutable!
- Aggregates are also not mutable! (series and sets for example)
- Math is hard enough, imagine if it was mutable!

# Pure Functions

- Pure functions take an immutable value and return an immutable value
- Pure functions always return the same result given the same input
- That is: they are 100% deterministic.
- This makes pure functions easy to understand, and easy to test.

# Types (and a brief aside about Haskell)

- ""avoid success at all costs" actually means "avoid (success at all costs)"

- Dynamic languages do not remove the need to reason about types in our programs
- Most static type systems for object oriented languages are fundamentally broken

		static int add(int a, int b) { ... }

- This looks safe enough ... but what if we actually look at the code?

		static int add(int a, int b) {
			nuke_france();
			return a + b;
		}


- They only specify the types of the input and the output values, but allow unrestricted side effects
- In a strongly typed functional language such as Haskell it is possible to express the following:

    -- this is a Haskell comment
    -- this function has type Integer to Integer
    -- from the type alone we know that calling this function will never have any side effects
    
    	add::Integer -> Integer -> Integer
    	add a b = a + b
    
    -- this function has type Unit to String
    -- that means that function takes no argument, modifies the outside world, and returns a String
    
    	add:: () -> IO String
    	add () = do 
    		_ <- unsafe_nuke_france
    		return "mission accomplished"
    	
- so what?
- when a function has side effects, you had better be certain about those side effects before you call it

# What Does It All Mean

- side effects are fucking dangerous
- side effects increase complexity
- side effects increase the probability of bugs
- side effects make it harder for other people and later versions of ourself to understand code
- side effects make it hard for us to ship new features

# How Does This Help In Practice?

- even in dyanimcally typed pervasively mutable object oriented languages separating side effects and purity has great value
- we don't have types for side effects in Javascript, Ruby, Objective-C, or Java!
- to apply this requires discipline and presence of mind
- take as a first principle to isolate code with side effects from pure code
- strive to organize our programs to be pure operations on data and operations with side effects that change the world

# Practical Problems

- most object languages do not provide immutable data structures
- there are libraries for most languages now that support these (immutable, hamster)
- most of our codebase is littered with side effects!
- life is hard

## But What About Performance?

- Immutable datastructures are slower
- When n is small, the difference is negligible
- Fortunately when n is large, researchers have been very clever

## So I Heard You Like Puzzles

- immutable hash tables

		hash = [] # the empty hash
		hash1 = hash.insert(:a, 42)
		-- let's say :a hashes to the number 1; we take the first n bits in the hash (which will equal 1) as the index 
		[0, 1]
		    \
		    [:a, 42] 
		    
		hash2 = hash1.insert(b:, 420)
		-- let's say :b hashes to the number 2; we take the first n bits in the hash (which will equal 1) as the index
		[0, 1, 2 .. 32]
		    \  \
		     \  [:b, 420]
		     [:a, 42]
		     
		hash3 = hash2.insert(c:, 99)
		-- let's say :c hashes to 33, so the first 5 bits are (00001) and the second five bits are (00001)
		[0, 1, 2 .. 32]
		    \  \
		     \  [:b, 420]
		      [0, 1, .. 32]
               \  \
                \  [:c, 420]
                 [:a, 42]
    
- this is just one example
- there has been a lot of research in this area for many years
- this stuff is ready to use and it it will make your life and the lives of your coworkers easier

# Further Resources
[Are We There Yet - Rich Hickey](http://www.infoq.com/presentations/Are-We-There-Yet-Rich-Hickey)

[QuakeCon Talk - John Carmack](https://www.youtube.com/watch?v=wt-iVFxgFWk)

[Red Black Trees - Chris Okasaki](https://wiki.rice.edu/confluence/download/attachments/2761212/Okasaki-Red-Black.pdf)

[Purely Functional Datastructures - Chris Okasaki](http://www.cs.cmu.edu/~rwh/theses/okasaki.pdf)

[Ideal Hash Trees - Phil Bagwell](http://lampwww.epfl.ch/papers/idealhashtrees.pdf)

[Object Oriented Programming Considered Harmful - some guy on the internet](http://www.iwriteiam.nl/AoP_OOCH.html)



	



