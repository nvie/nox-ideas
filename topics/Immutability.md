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


@mutates Tokenizer
func peek_token(self): Token {
  if (self.pos == self.tokens.size) {
    next = self.token_generator.next()
    tokens = self.tokens.push(next)
    self = tokenizer \\ { tokens }
  }

  return tokens[self.pos]
}


@mutates Tokenizer
func get_token(self, pos: Int): Token {
  // Because self.peek_token() is marked @mutates, this is sugar for:
  // mut (self, token) = self.peek_token()
  token = self.peek_token()
  self = self \\ { pos = pos + 1 }

  // Because this func is marked @mutates, this will implicitly return
  // a (Tokenizer, Token) tuple instead!
  return token
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


@mutates Parser
func expect(self: Parser, arg): Token? {
  // Because self.tokenizer.peek_token() is marked @mutates, this is implicitly
  // equivalent to:
  // mut (tokenizer, token) = self.tokenizer.peek_token()
  // self = self \\ { tokenizer }   // Because self is marked @mutates!

  token = self.tokenizer.peek_token()
  if (token.type == arg || token.string == arg) {
    // Because tokenizer.get_token() is marked @mutates, this is implicitly
    // equivalent to:
    // mut (tokenizer, token) = self.tokenizer.get_token()
    // self = self \\ { tokenizer }   // Because self is marked @mutates!
    return self.tokenizer.get_token()
  }
  return Nothing
}
```

