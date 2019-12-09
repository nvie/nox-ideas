# Immutability

Immutable data structures are the default.  However, this poses interesting
challenges regarding pragmatically expressing nested data mutations.  For
example, consider this pragmatic -- and relatively simple -- nesting of classes
that mutate their internal state as a side-effect.

https://medium.com/@gvanrossum_83706/building-a-peg-parser-d4869b5958fb#9ded

Translating this into Nox shows how involved the data mutations and function
signatures still are:

```nox
type Tokenizer = {
  token_generator: Iterable<Token>,
  tokens: Array<Token>,
  pos: Int,
}

@on Tokenizer
func mark(tok: Tokenizer) {
  return tok.pos;
}

@on Tokenizer
func reset(tok: Tokenizer, pos: Int): Tokenizer {
  return tok \\ { pos };
}

@on Tokenizer
func peek_token(tokenizer: Tokenizer): (Tokenizer, Token) {
  mut out = self

  if (self.pos == self.tokens.size) {
    next = self.token_generator.next()
    tokens = self.tokens.push(next)
    out = tokenizer \\ { tokens }
  }

  return (out, tokens[self.pos])
}

@on Tokenizer
func get_token(tok: Tokenizer, oldpos: Int): (Tokenizer , Token) {
  token = tok.peek_token()
  pos = oldpos + 1
  return (tok \\ { token, pos }, token)
}

type Parser = {
  tokenizer: Tokenizer,
}

@on Parser
func mark(self: Parser) {
  return self.tokenizer.mark()
}

@on Parser
func reset(self: Parser) {
  return self.tokenizer.reset()
}

@on Parser
func expect(self: Parser, arg): (Parser, Token?) {
  mut { tokenizer } = self
  (tokenizer, token) = tokenizer.peek_token()
  if (token.type == arg || token.string == arg) {
    (tokenizer, token) = self.tokenizer.get_token()
    out = self \\ { tokenizer }
    return (out, token)
  }
  return (self, Nothing)
}
```

