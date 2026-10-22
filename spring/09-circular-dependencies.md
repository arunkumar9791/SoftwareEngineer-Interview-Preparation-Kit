# Circular Dependencies

Trying to inject Bean A into Bean B while Bean B requires Bean A inside the constructor, breaks the lifecycle flow.

It is usually resolved by rewriting the structure to use setter / field injection or applying `@Lazy` annotation.
