In class Tuesday I wrote the following six triples on the board,
represented as a graph with two kinds of nodes (`(resources)` and
`[literal values]`) linked by `-- predicates -->`.

```
 1 (Ryan) -- givenName --> [Ryan]
 2 (Ryan) -- familyName--> [Shaw]
 3 (Ryan) -- holdsAccount --> (Ryan's twitter account)
 4 (Ryan) -- workplaceHomepage --> (SILS)
 5 (Ryan's twitter account) -- accountName --> [rybesh]
 6 (Ryan's twitter account) -- accountServiceHomepage --> (Twitter)
```

Recall that I said that predicates in RDF can have a *domain* and a
*range*. A predicate's domain tells us what we can infer about the
subjects of triples in which that predicate is used, while a
predicate's range tells us what we can infer about the objects of
triples in which that predicate is used. Based on how the domains
and ranges of some of the FOAF predicates are defined, we can also
infer the following four triples:

```
 7 (Ryan) -- type --> (Person)
 8 (Ryan's twitter account) -- type --> (OnlineAccount)
 9 (SILS) --type--> (Document)
10 (Twitter) --type--> (Document)
```

So, how to express these triples in Turtle?

Let's start with the first triple. The subject of this triple is me,
Ryan. Anything that can be the subject of a triple is a resource, so
I am a resource, and in Turtle, resources are referred to by their
globally unique identifiers--their URIs. So, what's my URI? Well, I
could use the URI of my homepage: `<https://aeshin.org>`. (The URI is
placed inside angle brackets `<>` to make it clear where it starts and
ends.)

Now for the predicate, `givenName`. As I mentioned in class, in RDF
predicates are also considered resources, so in Turtle they too are
referred to by their URIs. There is a formula for obtaining the URI
of any predicate: just take the *namespace* of the vocabulary in
which the predicate is defined, and append to it the name of the
predicate (in this case, `givenName`).

We know that `givenName` is defined by the FOAF vocabulary, but what
is the FOAF namespace? We could Google it, but the easiest thing to do
is to go to [Linked Open Vocabularies](https://lov.linkeddata.es), a catalog of RDF
vocabularies. Clicking on the large FOAF circle in the LOV diagram
will take you to [the LOV vocab page for FOAF](https://lov.linkeddata.es/dataset/lov/vocabs/foaf), where the namespace
of the vocabulary is given as `http://xmlns.com/foaf/0.1/`. (If we
pasted that namespace into a browser address bar, it would take us to
the FOAF documentation.) So the URI for the predicate `givenName` is
`<http://xmlns.com/foaf/0.1/givenName>`.

Finally, in Turtle literal values are put in double quotes, like
this: `"Ryan"`.

To write a triple in Turtle, I simply write the subject, then some
space (how much doesn't matter), then the predicate, than some space,
and then the object, then some space, and finally a period (to end the
statement): `S P O .` So, I would write the first triple like this:

```turtle
<https://aeshin.org>
  <http://xmlns.com/foaf/0.1/givenName>
  "Ryan"
  .
```

Following that same pattern for the rest of my triples, I get:

```turtle
<https://aeshin.org>
  <http://xmlns.com/foaf/0.1/givenName>
  "Ryan"
  .
<https://aeshin.org>
  <http://xmlns.com/foaf/0.1/familyName>
  "Shaw"
  .
<https://aeshin.org>
  <http://xmlns.com/foaf/0.1/holdsAccount>
  <https://twitter.com/rybesh>
  .
<https://aeshin.org>
  <http://xmlns.com/foaf/0.1/workplaceHomepage>
  <https://sils.unc.edu>
  .
<https://twitter.com/rybesh>
  <http://xmlns.com/foaf/0.1/accountName>
  "rybesh"
  .
<https://twitter.com/rybesh>
  <http://xmlns.com/foaf/0.1/accountServiceHomepage>
  <https://twitter.com>
  .
```
 
How about the extra four triples that I inferred? The `type` predicate
is not part of the FOAF vocabulary. Instead it is defined by the core
RDF vocabulary. We can find the namespace for the core RDF vocabulary
by going to [the LOV vocabs page](https://lov.linkeddata.es/dataset/lov/vocabs) and [searching for `rdf`](https://lov.linkeddata.es/dataset/lov/vocabs?q=rdf). The
first result is [the LOV catalog page for the core RDF vocabulary](https://lov.linkeddata.es/dataset/lov/vocabs/rdf),
which tells us that the namespace is
`http://www.w3.org/1999/02/22-rdf-syntax-ns#`. So, the URI of the
`type` predicate is
`<http://www.w3.org/1999/02/22-rdf-syntax-ns#type>`:

```turtle
<https://aeshin.org>
  <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
  <http://xmlns.com/foaf/0.1/Person>
  .
<https://twitter.com/rybesh>
  <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
  <http://xmlns.com/foaf/0.1/OnlineAccount>
  .
<https://sils.unc.edu>
  <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
  <http://xmlns.com/foaf/0.1/Document>
  .
<https://twitter.com>
  <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
  <http://xmlns.com/foaf/0.1/Document>
  .
```

The objects of these inferred triples are also resources (as we can
tell since they are represented by URIs in angle brackets). But what
kind of resources, and where did their URIs come from? They look
like the URIs for predicates, but they can't be predicates, since
they are listed last in the triples in which they appear.

The answer is that they are other concepts defined by the FOAF
vocabulary. These concepts are used to classify the kinds of things
describable using the FOAF vocabulary, and so they are referred to
as *classes*. The URIs for classes are constructed exactly like the
URIs for predicates: the name of the class is appended to the
vocabulary namespace. By convention, the names of classes are
capitalized, and the names of predicates are not.

OK, so now we can write triples in Turtle, but they're not so
pleasant to look at. Using URIs to refer to almost everything
results in a lot of redundancy and visual noise in the
file. Fortunately Turtle has a mechanism to address this:
prefixes. Using a special "triple" (it isn't really, but it looks
like one) at the top of the file, we can define an abbreviation for
a namespace. This abbreviation is known as a *prefix*, and the
triple defining it looks like this:

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
```

Now, whenever we have a URI starting with
`http://xmlns.com/foaf/0.1/`, we can replace that part with `foaf:`,
and drop the angle brackets:

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

<https://aeshin.org> foaf:givenName "Ryan" .

<https://aeshin.org> foaf:familyName "Shaw" .

<https://aeshin.org> foaf:holdsAccount <https://twitter.com/rybesh> .

<https://aeshin.org> foaf:workplaceHomepage <https://sils.unc.edu> .

<https://twitter.com/rybesh> foaf:accountName "rybesh" .

<https://twitter.com/rybesh> foaf:accountServiceHomepage <https://twitter.com> .
```

The result is easier to read, and we can now fit each triple on one
line. We can do the same with the RDF namespace to shorten our
inferred triples:

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

<https://aeshin.org> rdf:type foaf:Person .

<https://twitter.com/rybesh> rdf:type foaf:OnlineAccount .

<https://sils.unc.edu> rdf:type foaf:Document .

<https://twitter.com> rdf:type foaf:Document .
```

But, it isn't actually necessary to do this, because the `rdf:type`
predicate is so common that Turtle has a built-in abbreviation for it:
the single character `a`. So the triples above can also be written as:

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

<https://aeshin.org> a foaf:Person .

<https://twitter.com/rybesh> a foaf:OnlineAccount .

<https://sils.unc.edu> a foaf:Document .

<https://twitter.com> a foaf:Document .
```

The Turtle looks a lot easier to read now, but there is still some
redundancy where several triples have the same subject. Turtle has a
feature to address this too: multiple triples with the same subject
but different predicates and objects can be written in the form `S P O
; P O ; P O .`. Doing this neatly groups together triples by subject:

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

<https://aeshin.org>
  foaf:givenName "Ryan" ;
  foaf:familyName "Shaw" ;
  foaf:holdsAccount <https://twitter.com/rybesh> ;
  foaf:workplaceHomepage <https://sils.unc.edu>
  .
<https://twitter.com/rybesh>
  foaf:accountName "rybesh" ;
  foaf:accountServiceHomepage <https://twitter.com>
  .
```

Adding our inferred triples, we get:

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

<https://aeshin.org>
  a foaf:Person ;
  foaf:givenName "Ryan" ;
  foaf:familyName "Shaw" ;
  foaf:holdsAccount <https://twitter.com/rybesh> ;
  foaf:workplaceHomepage <https://sils.unc.edu>
  .
<https://twitter.com/rybesh>
  a foaf:OnlineAccount ;
  foaf:accountName "rybesh" ;
  foaf:accountServiceHomepage <https://twitter.com>
  .

<https://sils.unc.edu> a foaf:Document .
<https://twitter.com> a foaf:Document .
```

Very readable, and minimum redundancy.

If multiple triples have *both* the same subject and the same
predicate, they can be written in the form `S P O , O , O .` For
example, if I wanted to add a triple asserting my given name in
Japanese, I could write:

```turtle
<https://aeshin.org> foaf:givenName "Ryan" , "ライアン" .
```

This is equivalent to separately writing the two triples:

```turtle
<https://aeshin.org> foaf:givenName "Ryan" .
<https://aeshin.org> foaf:givenName "ライアン" .
```

Finally--what if you want to refer to things for which you can't find
suitable URIs? For example, I have my own domain name (`aeshin.org`),
so it is easy for me to use that to refer to myself. But what if I
didn't? Well, in that case I could use a *relative URI*. A relative
URI is incomplete: to construct the full URI for something, one takes
the URI of the file in which the relative URI appears, and appends the
relative URI to it. The author of the file doesn't need to worry about
what the URI of the file will be, at least while they're authoring the
file. They can just use the relative URI.

A common convention is to use relative URIs that start with a hash
mark: `#`. (We'll learn why later in the semester.) So if I wanted to
use a relative URI to refer to myself in this file, I could use
`<#me>`. Everything else would remain the same:

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

<#me>
  a foaf:Person ;
  foaf:givenName "Ryan" , "ライアン" ;
  foaf:familyName "Shaw" ;
  foaf:holdsAccount <https://twitter.com/rybesh> ;
  foaf:workplaceHomepage <https://sils.unc.edu>
  .
<https://twitter.com/rybesh>
  a foaf:OnlineAccount ;
  foaf:accountName "rybesh" ;
  foaf:accountServiceHomepage <https://twitter.com>
  .

<https://sils.unc.edu> a foaf:Document .
<https://twitter.com> a foaf:Document .
```
