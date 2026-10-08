# Osysharp.Memory.Testing

**Test search by meaning without a model.** A deterministic, offline embedder for your test suite, so you can assert
what `Memory.Search` and `[Searchable(Memory)]` find — and, just as important, what they never find.

## Use it

```osy
app Notes {
  use Osysharp.Memory;
  tests use Osysharp.Memory.Testing@0;   // loaded by `osy test` alone — never part of the app you deploy
  model "model/**/*.osy";
  tests "tests/**/*.test.osy";
}
```

```osy
using Osysharp.Memory;
using Osysharp.Memory.Testing;

[TestFixture]
void Garden() {
  TestEmbedder.Use(new WordsEmbedder());
  new FieldNote { Title = "Tomatoes", Body = "The tomatoes split after heavy rain; water them evenly." };
}

[Test(Garden)]
void a_note_is_found_by_what_it_says() {
  var hit = Memory.Search("tomatoes split rain", of: [FieldNote], limit: 1).Single();
  Assert.Equal("Tomatoes", FieldNote.Single(n => n.Id == hit.SourceEntityId).Title);
}
```

## What you get

- **Rows found on the next line.** Under `TestEmbedder.Use`, every searchable row is embedded as it is committed —
  no background worker to wait for.
- **The same answer everywhere.** `WordsEmbedder` is a hashed bag of words: two texts are as close as the words they
  share. Free, offline, identical on every machine and every run.
- **Your own, when you need meaning.** Any class implementing `IEmbedder` can be used — a small table of words that mean
  the same is often enough to test search across synonyms or languages.
- **Only ever in a test.** `TestEmbedder.Use` is refused outside a `[Test]` or `[TestFixture]`, and on any run that is
  not a test.

`WordsEmbedder` is lexical, not semantic — "invoice" is not near "bill" — so search with words your rows contain, and
assert which rows are found, in which order, and which never are.

## Its tests

A note found by its words, the closest first, a private note never found by anyone but its keeper, a note written in
the test found at once, and the same text always the same vector.
