# Redefining Stability

> Published on 2016-08-31

Defining stability is a hard problem due to many factors. I propose the use of
some guidelines and modifiers to describe various "stable" systems to better
describe their true state.

## Common Points

- The software runs within its defined memory/cpu parameters
- The software does not crash under "normal" load

## Understable

- There is some risk in using the service. Not all edge cases for the API have
  been tested/bugfixed
- Deployments need monitoring by "that one person" who understands the service
  deeply
- Subject to large or frequent code / API updates
- Using "new" or untested libraries, repeatedly updated (subject to frequent
  deployments)

## Stable

- Most edge-cases have been fixed for the API
- Engineering teams maintain and update the code regularly (as needed or
  monthly)
- Relatively few updates to the business-logic code. The API is versioned and/or
  tested for backwards compatibility
- Libraries are updated and the app is refactored to keep pace with security
  updates and Engineering progress outside the app (e.g. database version
  changes)

## Overstable

- So "rock solid" that no one wants to touch it -- it has "hairs" on it in the
  form of bugfixes and all edge-cases have been handled in code.
- Code is reviewed / updated only when absolutely necessary to prevent
  catastrophic failures. Library and code updates are avoided.
- Would be considered "abandonware" in other contexts
- (Like *understable*) requires "that one person" who understands the code to
  maintain it

## Conclusions and Reasoning

Both under- and over-stable systems are in a "bad" state and are something we
should avoid.

Having truly Stable code means regularly reviewing or refactoring code to
account for new Engineering requirements or practices, fixing or adding pieces
as needed, and keeping the team's understanding of the code fresh.
