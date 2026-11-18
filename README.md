# Scala vs Mercury Comparison Examples

This project demonstrates key differences and similarities between Scala and Mercury programming languages through practical examples.

## Critical Analysis: Strengths, Weaknesses, and Practical Considerations

### Scala: Strengths and Weaknesses

#### Strengths

**Flexibility and Expressiveness:** Scala's multi-paradigm nature allows developers to choose the most appropriate style for each problem. As Odersky, Spoon & Venners (2016) note, Scala was designed as a "unification of object-oriented and functional programming" that operates at a "higher level of abstraction" than Java (Scala 3.4 Language Specification, Section 6). This flexibility makes Scala well-suited for teams with diverse skill backgrounds.

**Ecosystem Integration:** Running on the JVM provides immediate access to vast libraries, mature debugging tools, and enterprise infrastructure. The JVM's just-in-time compilation optimizes hot code paths at runtime, often resulting in performance competitive with or exceeding hand-optimized C code for many workloads (Sebesta, 2019).

**Gradual Learning Curve:** Scala allows incremental adoption of advanced features. Developers can start with familiar imperative patterns and gradually introduce functional concepts like higher-order functions and monads as their comfort grows. The Scala 3 Book notes that this "step-by-step" approach makes it accessible while still providing power for complex systems.

#### Weaknesses

**Complexity Overhead:** Scala's expressive power comes at the cost of conceptual complexity. Features like implicit parameters, complex type inference, and advanced type system constructs (higher-kinded types, context bounds) create significant cognitive load for newcomers. Sebesta (2019) observes that "the power and flexibility of Scala's type system is also its greatest weakness, as it can lead to code that is difficult to understand and maintain."

**Performance Variability:** While the JVM optimizes hot paths well, Scala programs can suffer from unpredictable performance characteristics due to:
- Heap allocation overhead for closures and function objects
- Memory pressure from immutable data structures
- Garbage collection pauses affecting latency-sensitive applications

**Compilation Time:** Scala's sophisticated type system requires extensive compilation analysis, leading to longer compile times than many languages. For large projects, this can significantly impact developer iteration speed.

### Mercury: Strengths and Weaknesses

#### Strengths

**Guaranteed Determinism:** Mercury's mode and determinism system provides compile-time guarantees about program behavior that are impossible to express in most other languages. As the Mercury Reference Manual states, "the compiler enforces these declarations," preventing entire classes of runtime errors at compile time (Nelson, 1991).

**Optimal Memory Management:** The combination of single assignment and a sophisticated garbage collector enables aggressive optimizations that can outperform languages with mutable state. The region-based memory management research by Mazur (2004) demonstrated up to 25% performance improvements for certain workloads.

**Reliability for Critical Systems:** Mercury's purity guarantees mean functions cannot have hidden side effects, making code easier to reason about and test. This is particularly valuable for safety-critical systems where correctness is paramount.

#### Weaknesses

**Steep Learning Curve:** Mercury's paradigm shift from procedural/imperative thinking to logic programming requires significant mental adjustment. The mode system, while powerful, is conceptually challenging for programmers unfamiliar with formal logic or Prolog. Fondren (2016) notes that "the mode system is one of the hardest concepts for newcomers to grasp."

**Limited Ecosystem:** Mercury lacks the vast library ecosystem and tooling of mainstream languages like Scala. This makes it impractical for many real-world applications, particularly those requiring:
- GUI development
- Web framework support
- Database connectivity libraries
- Networking frameworks

**Niche Adoption:** Mercury's small community means fewer resources, slower answers to questions, and limited job opportunities. This creates a self-reinforcing cycle where practical considerations outweigh technical advantages.

### Comparative Analysis: Which Language Handles Which Features Better?

| Feature | Better Language | Why |
|---------|-----------------|-----|
| **Closures/Higher-order** | Scala | JVM's mature closure implementation with efficient heap allocation and JIT optimization. Mercury requires explicit mode declarations for higher-order terms (The Mercury Language Reference Manual) |
| **State Management** | Mercury | Single assignment eliminates entire classes of bugs. Scala's mutable `var` provides flexibility but risks side effects (Odersky et al., 2016) |
| **Error Handling** | Mercury | Mode system catches errors at compile time; no runtime exceptions for type or mode violations. Scala relies on try-catch and Option types, which can still leak to runtime |
| **Performance Predictability** | Mercury | Deterministic execution means predictable timing behavior. Scala's JIT compilation introduces variability, with some paths optimized and others not |
| **Expressiveness** | Scala | Rich type system with implicits, type classes, and advanced FP constructs enable very concise solutions (Scala 3.4 Language Specification) |
| **Learning Accessibility** | Scala | Developers can start with familiar patterns and gradually adopt functional concepts. Mercury requires adopting a fundamentally different programming paradigm from day one |

### Reflection on Ease of Learning

**Scala's Approachability:** Scala was designed with adoption in mind. Its syntax is similar to Java, making it accessible to enterprise developers. The language provides "escape hatches" allowing imperative style when functional approaches feel awkward. However, mastering Scala requires understanding:
- The distinction between methods and function values (Odersky et al., 2016)
- Implicit conversions and parameters
- Advanced type system features

**Mercury's Conceptual Hurdles:** Mercury demands immediate engagement with:
- Logic programming paradigm (predicates vs. functions)
- Mode system (`in`, `out`, `di`, `uo`)
- Determinism declarations (`det`, `semidet`, etc.)
- Backtracking semantics

The learning curve is steeper, but once mastered, the compile-time guarantees provide confidence in code correctness that's difficult to match in other languages.

### Practical Use Cases

**Scala Excels At:**
- Backend services and APIs (Akka, Play Framework)
- Data processing pipelines (Spark, fs2)
- Applications requiring JVM ecosystem integration
- Teams needing gradual functional adoption

**Mercury Shines When:**
- Formal verification is required (e.g., safety-critical systems)
- Compile-time correctness guarantees are more important than development speed
- Research or educational settings teaching logic programming concepts
- Applications where deterministic execution timing is critical

## Language Overview & Architecture

### Scala: Multi-Paradigm Powerhouse
Scala blends object-oriented and functional programming into a single cohesive language. It runs on the JVM and offers:
- **Flexible type system**: Supports both immutable (`val`) and mutable (`var`) state
- **Expressions everywhere**: `if` is an expression that returns values
- **Rich collection API**: For-comprehensions, pattern matching with guards
- **Sealed traits & Option**: Exhaustive pattern matching with compile-time safety

### Mercury: Logic Programming Purity
Mercury is a purely functional logic language designed for correctness:
- **Single assignment**: All variables are immutable by default
- **Mode system**: Compile-time determinism guarantees (`is det`, `is semidet`)
- **Predicates & functions**: Separate constructs with explicit input/output modes
- **No side effects**: Purity enforced at the type level

| Feature | Scala | Mercury |
|---------|-------|---------|
| Paradigm | Multi-paradigm (OOP + FP) | Logic/Functional hybrid |
| Typing | Static, flexible | Static, Hindley-Milner inference |
| State | `val` (immutable) / `var` (mutable) | Single assignment only |
| Control | Expressions (if), loops | Recursion, if-then-else |
| Error Handling | Try-catch-finally | Mode system + failing predicates |

## Running the Examples

### GitHub Actions CI/CD Workflow

The project includes a GitHub Actions workflow (`test-scala-mercury.yml`) that automatically runs on:
- Pushes to `main` or `develop` branches
- Pull requests targeting those branches
- Manual triggers via "Run workflow" button

**What happens:**
1. Repository is checked out
2. Docker container is built from the language-specific Dockerfile
3. Compiler version is confirmed inside the container
4. All examples are compiled and executed
5. Results are logged for review

### Running Locally (Docker)

You can run the same tests locally using Docker:

**Scala:**
```bash
# Build the container
docker build -t scala-test:latest -f jeff/scala/Dockerfile.scala .

# Run all tests
docker run --rm scala-test:latest bash /app/jeff/scala/test-scala.sh
```

**Mercury:**
```bash
# Build the container
docker build -t mercury-test:latest -f jeff/mercury/Dockerfile.mercury .

# Run all tests
docker run --rm mercury-test:latest bash /app/jeff/mercury/test-mercury.sh
```

### Running Manually (Inside Container)

For interactive debugging, start a shell in the container:

```bash
# Scala interactive shell
docker run -it --rm scala-test:latest bash
# Then inside:
cd /app/jeff/scala
scalac *.scala
scala scala_subprograms.scala

# Mercury interactive shell  
docker run -it --rm mercury-test:latest bash
# Then inside:
cd /app/jeff/mercury
mmc --make *.m
./mercury_subprograms
```

## File Structure

```
jeff/
├── README.md                          # This file
├── SUBPROGRAMS.md                     # Detailed analysis of subprograms
└── REFERENCES.md                      # Citations in MLA format
├── scala/
│   ├── Dockerfile.scala               # Container build for Scala
│   ├── scala_data_types_or_binding.scala
│   ├── scala_control_structures.scala
│   └── scala_subprograms.scala
└── mercury/
    ├── Dockerfile.mercury             # Container build for Mercury
    ├── mercury_data_types_or_binding.m
    ├── mercury_control_structures.m
    └── mercury_subprograms.m
```

## Requirements

- **Docker**: Required for running containers (free tier available)
- No local Scala/Mercury installation needed - everything is containerized

