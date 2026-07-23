# backgammon

A pure-Go backgammon rules engine — no protocol, no I/O, no dependencies
beyond the standard library.

The core is **legal-turn generation**: `LegalTurns` enumerates every complete
legal turn for a position and dice roll, encoding the fiddly tabletop
obligations (you must use both dice if you can; if only one is playable, it
must be the larger; bar entry comes first; bear-off exact/overshoot rules)
as *set membership* rather than a pile of special cases. `Validate` then just
checks a submitted turn against that set — ideal for both-sides-validate
multiplayer where neither client is trusted.

```go
b := backgammon.Start()
turns := backgammon.LegalTurns(b, backgammon.White, 3, 1)
// pick one, validate it, apply it
if err := backgammon.Validate(b, backgammon.White, 3, 1, turns[0]); err == nil {
    b = backgammon.ApplyTurn(b, backgammon.White, turns[0])
}
result := b.Winner() // nil until someone bears off all 15 (plain/gammon/backgammon)
```

Points are numbered 1–24 in White's frame (Black's point *p* is `25-p`);
moves use each player's own numbering. Bar is `From: 25`, bear-off is
`To: 0`. See the doc comments for details.

**Not yet implemented:** the doubling cube.

## Install

```sh
go get github.com/richardwooding/backgammon
```

Extracted from [kibitz](https://github.com/richardwooding/kibitz), where it
drives E2E-encrypted online backgammon.

## License

MIT
