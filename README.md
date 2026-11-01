# Osysharp.Memory

stated links between records, files remembered about a record, and the operator's statistics and prune — the memory verbs over the platform's vector store.

## Examples

Taken from this package's own tests, which compile and run on every release.

```osy
using Osysharp.Memory;

[Test(Desk)]
[runas(Ada)]
void a_link_is_stated_and_withdrawn_from_either_end() {
  var q1 = Deal.Single(d => d.Title == "Q1 deal");
  var q3 = Deal.Single(d => d.Title == "Q3 deal");
  Assert.True(Supersede(q1, q3));
  UnitOfWork.Commit();
  // Which end was the source is storage: named the other way round, it is the same link.
  Assert.Equal(1, Unrelate(q3, q1));
  UnitOfWork.Commit();
  Assert.Equal(0, Unrelate(q1, q3));
}
```

From [`tests/link.test.osy`](tests/link.test.osy).

```osy
using Osysharp.Memory;

[Test(Gate)]
[runas(Ops)]
void the_operator_may_read_what_memory_costs() {
  var usage = MemoryUsage(90);
  Assert.True(usage.Authored <= usage.Entries);
  Assert.True(usage.Reclaimable <= usage.Derived);
}
```

From [`tests/operator.test.osy`](tests/operator.test.osy).

```osy
using Osysharp.Memory;
using Osysharp.Storage;

[Test(Files)]
[runas(Ada)]
void a_file_never_remembered_has_nothing_to_forget() {
  var terms = new FileAsset { Name = "terms.txt", MimeType = "text/plain" };
  UnitOfWork.Commit();
  Assert.Equal(0, Drop(terms));
  // Forgetting is about recall, not storage: the file is still there.
  Assert.Equal(1, FileAsset.Count(f => f.Name == "terms.txt"));
}
```

From [`tests/remember.test.osy`](tests/remember.test.osy).

## Learn more

- [Osysharp.Memory in the package catalogue](https://osysharp.com/packages/#osysharp-memory)
- [The Osy# reference](https://osysharp.com/reference/) — the language, the runtime and every kit
- [`llms-full.txt`](https://osysharp.com/llms-full.txt) — the whole reference and specification as one Markdown file, for a coding agent

## Licence

MIT — see [`LICENSE`](LICENSE).
