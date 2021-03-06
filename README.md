# Peroxide

[![On crates.io](https://img.shields.io/crates/v/peroxide.svg)](https://crates.io/crates/peroxide)
[![On docs.rs](https://docs.rs/peroxide/badge.svg)](https://docs.rs/peroxide/)
[![travis](https://api.travis-ci.org/Axect/Peroxide.svg?branch=master)](https://travis-ci.org/Axect/Peroxide)  
![maintenance](https://img.shields.io/badge/maintenance-actively--developed-brightgreen.svg)

Pure Rust numeric library contains linear algebra, numerical analysis, statistics and machine learning tools with R, MATLAB, Python like macros.

## Latest README version

Corresponds with `0.7.3`.

## Install

* Add next line to your `cargo.toml`
```toml
peroxide = "0.7"
```

## Usage

There is [Peroxide Guide](http://nbviewer.jupyter.org/github/Axect/Example_Peroxide/blob/master/Peroxide%20Guide.ipynb)
written on Jupyter.

### Initial Import

```rust
extern crate peroxide;
use peroxide::*;
```

### Module Structure

- __src__
  - __bin__ : For test some libraries
    - [dual.rs](src/bin/dual.rs) : Test automatic differentiation
    - [oxide.rs](src/bin/oxide.rs) : Test any
    - [poly.rs](src/bin/poly.rs) : Test polynomial
  - [lib.rs](src/lib.rs) : `mod` and `re-export`
  - __ml__ : For machine learning
    - [mod.rs](src/ml/mod.rs)
    - [reg.rs](src/ml/reg.rs) : Regression tools
  - __macros__ : Macro files
    - [matlab_macro.rs](src/macros/matlab_macro.rs) : MATLAB like macro
    - [mod.rs](src/macros/mod.rs)
    - [r_macro.rs](src/macros/r_macro.rs) : R like macro
  - __numerical__ : To do numerical things
    - [interp.rs](src/numerical/interp.rs) : Interpolation
    - [mod.rs](src/numerical/mod.rs)
    - [spline.rs](src/numerical/spline.rs) : Spline
  - __operation__ : To define general operations
    - [extra_ops.rs](src/operation/extra_ops.rs)
    - [mod.rs](src/operation/mod.rs)
  - __statistics__ : Statistical Tools
    - [mod.rs](src/statistics/mod.rs)
    - [rand.rs](src/statistics/rand.rs) : Wrapper for `rand` crate
    - [stat.rs](src/statistics/stat.rs) : Statisitcal tools
  - __structure__ : Fundamental data structures
    - [dual.rs](src/structure/dual.rs) : Dual number system for automatic differentiation
    - [matrix.rs](src/structure/matrix.rs) : Matrix
    - [mod.rs](src/structure/mod.rs)
    - [polynomial.rs](src/structure/polynomial.rs) : Polynomial
    - [vector.rs](src/structure/vector.rs) : Extra tools for `Vec<f64>`
  - __util__
    - [mod.rs](src/util/mod.rs)
    - [non_macro.rs](src/util/non_macro.rs) : Primordial version of macros
    - [print.rs](src/util/print.rs) : To print conveniently

### Vec\<f64\> Declaration

```R
# R
a = c(1,2,3,4)
b = seq(1,5,2) # (=c(1,3,5))
```

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = c!(1,2,3,4);
    let b = seq!(1,5,2); // (=c!(1,3,5))
}
```

### Matrix Declaration

```R
# R
a = matrix(1:4, 2, 2, T)
```

```rust
// Peroxide (All belows are same)
extern crate peroxide;
use peroxide::*;

fn main() {
    // matrix function
    let a = matrix(vec![1,2,3,4], 2, 2, Row);
    let b = matrix(c!(1,2,3,4), 2, 2, Row);
    let c = matrix(seq!(1,4,1), 2, 2, Row);
    
    // matrix macro (More convenient)
    let d = matrix!(1;4;1, 2, 2, Row);
    
    // Pythonic
    let p_mat = p_matrix(vec![c!(1, 2), c!(3, 4)]);
    
    // Matlab like
    let m_mat = m_matrix("1 2; 3 4");
}
```

### Print

```R
# R
a = matrix(1:4, 2, 2, T)
print(a)
#      [,1] [,2]
# [1,]    1    2
# [2,]    3    4
```

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = matrix!(1;4;1,  2, 2, Row);
    println!("{}", a);
    // Or
    a.print(); // You can print number, vector, matrix as same way
}
//       c[0] c[1]
// r[0]     1    2
// r[1]     3    4
```


### Concatenate

**1. Vector + Vector => Vector**
```R
# R
a = c(1,2,3)
b = c(4,5,6)

c = c(a, b) 
print(c) # c(1,2,3,4,5,6)
```

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = c!(1,2,3);
    let b = c!(4,5,6);
    let c = c!(a; b); // Must use semi-colon btw vectors
    c.print();
}
```

**2. Matrix + Matrix => Matrix**
```R
# R
# cbind
a = matrix(1:4, 2, 2, F)
b = matrix(c(5,6), 2, 1, F)
c = cbind(a, b)
print(c)
#     [,1] [,2] [,3]
#[1,]    1    3    5
#[2,]    2    4    6

# rbind
a = matrix(1:4, 2, 2, T)
b = matrix(c(5,6), 1, 2, T)
c = rbind(a,b)
print(c)
#     [,1] [,2]
#[1,]    1    2
#[2,]    3    4
#[3,]    5    6
```

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    // cbind
    let a = matrix!(1;4;1, 2, 2, Col);
    let b = matrix(c!(5,6), 2, 1, Col);
    let c = cbind!(a, b);
    c.print();
    //      c[0] c[1] c[2]
    // r[0]    1    3    5
    // r[1]    2    4    6
    
    // rbind
    let d = matrix!(1;4;1, 2, 2, Row);
    let e = matrix(c!(5,6),1, 2, Row);
    let f = rbind!(a, b);
    f.print();
    //      c[0] c[1]
    // r[0]    1    2
    // r[1]    3    4
    // r[2]    5    6
}
```

### Matrix operation

* If you want to do multiple operations on same matrix, then you should use `clone` because Rust `std::ops` consume value. 

```R
# R
a = matrix(1:4, 2, 2, T)
b = matrix(1:4, 2, 2, F)
print(a + b)
print(a - b)
print(a * b)
print(a %*% b)
```

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = matrix!(1;4;1, 2, 2, Row);
    let b = matrix!(1;4;1, 2, 2, Col);
    println!("{}", a.clone() + b.clone());
    println!("{}", a.clone() - b.clone());
    println!("{}", a.clone() * b.clone()); // Element-wise multiplication
    println!("{}", a % b);  // Matrix multiplication
    // Consume -> You can't use a,b anymore.
}
```

### LU Decomposition

* Peroxide uses complete pivoting LU decomposition. - Very stable.
* Also there are lots of error handling for LU, so, you should use `Option`

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = matrix(c!(1,2,3,4), 2, 2, Row);
    let pqlu = a.lu().unwrap(); // for singular matrix, returns None
    let (p,q,l,u) = (pqlu.p, pqlu.q, pqlu.l, pqlu.u);
    assert_eq!(p, vec![(0,1)]); // swap 0 & 1 (Row)
    assert_eq!(q, vec![(0,1)]); // swap 0 & 1 (Col)
    assert_eq!(l, matrix(c!(1,0,0.5,1),2,2,Row));
    assert_eq!(u, matrix(c!(4,3,0,-0.5),2,2,Row));
}
```

### Determinant

* Determinant is implemented using by LU decomposition (O(n^3))

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = matrix(c!(1,2,3,4), 2, 2, Row);
    assert_eq!(a.det(), -2f64);
}
```

### Inverse

* Inverse is also implemented using by LU decomposition
* To handle singularity, output type is `Option<Matrix>`
    * To obtain inverse, you should use `unwrap` or pattern matching
    
```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    // Non-singular
    let a = matrix!(1;4;1, 2, 2, Row);
    assert_eq!(a.inv().unwrap(), matrix(c!(-2,1,1.5,-0.5),2,2,Row));
    
    // Singular
    let b = matrix!(1;9;1, 3, 3, Row);
    assert_eq!(b.inv(), None);
 }
 ```

### Extract Column or Row

```R
# R
a = matrix(1:4, 2, 2, T)
print(a[,1])
print(a[,2])
print(a[1,])
print(a[2,])
```

```rust
//Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = matrix!(1;4;1, 2, 2, Row);
    a.col(0).print();
    a.col(1).print();
    a.row(0).print();
    a.row(1).print();
}
```

### Functional Programming

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = matrix!(1;4;1, 2, 2, Row);
    println!("{}", a.fmap(|x| x + 1.0));
    println!("{}", a.fmap(|x| x - 1.0));
    println!("{}", a.fmap(|x| x * 2.0));
}

// Results
//
//       c[0] c[1]
// r[0]     2    3
// r[1]     4    5
//
//       c[0] c[1]
// r[0]     0    1
// r[1]     2    3
//
//       c[0] c[1]
// r[0]     2    4
// r[1]     6    8
```

### Write to CSV

You can write matrix to csv by two ways.

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;
use std::process; // for error handling

fn main() {
    // 1. Just write
    let a = matrix!(1;4;1, 2, 2, Row);
    a.write("test.csv"); // It will save a to test.csv

    // 2. Error Handling
    let b = matrix!(1;4;1, 2, 2, Row);
    if let Err(err) = b.write("test.csv") {
      println!("{}", err);
      process::exit(1);
    }
    
    // And also with header
    let c = m_matrix("1 2;3 4");
    let c_header = vec!["a", "b"];
    c.write_with_header("test_h.csv", c_header);
}
```

### Read from CSV

You can read matrix with error handling

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;
use std::process;

fn main() {
    let m = Matrix::read("test.csv", false, ','); // no header, delimiter = ','
    // Error handling
    match m {
        Ok(mat) => println!("{}", mat),
        Err(err) => {
            println!("{}", err);
            process::exit(1);
        }
    }
    
    // Just write
    let n = Matrix::read("test.csv", false, ',').unwrap(); // no header
    println!("{}", n);
}
```

### Statistics

* `mean` - Mean
* `var` - Variance
* `sd` - Standard Deviation
* `cov` - Covariance
* `cor` - Pearson's Coefficient

```r
# R
# Vector Stats
a <- c(1,2,3)
b <- c(3,2,1)
print(mean(a))
print(var(a))
print(sd(a))
print(cov(a, b))
print(cor(a, b))

# Matrix Stats
m <- matrix(c(1,2,3,3,2,1), 3, 2, F)
print(cov(m))
``` 

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    // Vector Stats
    let a = c!(1,2,3);
    let b = c!(3,2,1);
    
    println!("{}",a.mean());
    println!("{}",a.var());
    println!("{}",a.sd());
    println!("{}",cov(&a, &b)); // Should borrow! - Not consume value
    println!("{}",cor(&a, &b));
    
    // Matrix Stats
    let m = matrix(c!(1,2,3,3,2,1), 3, 2, Col);
    println!("{}",m.cov());
}
```

### Linear Regression

* `lm(x, y)`

```r
# R
a <- c(1,2,3,4,5)
b <- a + rnorm(5)
lm(b ~ a)

#Call:
#lm(formula = b ~ a)
#
#Coefficients:
#(Intercept)            a  
#     0.5076       0.8305  
```

```rust
//Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = c!(1,2,3,4,5).to_matrix();
    let b = a.clone() + Normal(0,1).sample(5).to_matrix();
    lm!(b ~ a).print();
    
    //        c[0]
    // r[0] 0.7219
    // r[1] 0.8058
}

```

### Probability Distributions

Current available distribution

* `TPDist`: Two parameter distributions
    * `Uniform(start, end)` (Wrap `rand` crate)
    * `Normal(mean, std)` (Using ziggurat algorithm)
* Available methods
    * `sample(n: usize) -> Vec<f64>`: extract sample
    * `pdf(x: f64) -> Vec<f64>`: calculate probability at `x`

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    // Uniform unsigned integers
    let v_u32 = Uniform(1u32, 11);
    v_u32.sample(10).print();
    
    // Uniform 64bit float numbers
    let v_f64 = Uniform(1f64, 11f64);
    v_f64.sample(10).print();
    
    // Normal distribution with mean 0, sd 1
    let v_n = Normal(0, 1);
    println!("{}", v_n.sample(1000).mean()); // almost 0
    println!("{}", v_n.sample(1000).sd()); // almost 1
    
    // Probability of normal dist at x = 0
    v_n.pdf(0.).print(); //0.3989422804014327
}
```

### Polynomial

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    // Declare polynomial
    let a = poly(c!(1,3,2));
    a.print(); // x^2 + 3x + 2
    a.eval(1); // Evaluate when x = 1 -> 6.0
    
    let b = poly(c!(1,2,3,4));       // x^3 + 2x^2 + 3x + 4
    (a.clone() + b.clone()).print(); // x^3 + 3x^2 + 6x + 6
    (a.clone() - b.clone()).print(); // -x^3 - x^2 - 2
    (a.clone() * b.clone()).print(); // x^5 + 5x^4 + 11x^3 + 17x^2 + 18x + 8
    
    let c = poly(c!(1, -1));
    c.print();                       // x - 1
    c.pow(2).print();                // x^2 - 2x + 1
}
```

### Interpolation (Beta)

* Lagrange polynomial interpolation
* Chebyshev nodes

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = c!(-1, 0, 1);
    let b = c!(1, 0, 1);

    let l = lagrange_polynomial(a, b);
    l.print(); // x^2
}
```

### Spline (Beta)

* Natural cubic spline

```rust
// Peroxide
extern crate peroxide;
use peroxide::*;

fn main() {
    let x = c!(0.9, 1.3, 1.9, 2.1);
    let y = c!(1.3, 1.5, 1.85, 2.1);

    let s = cubic_spline(x, y);

    for i in 0 .. s.len() {
        s[i].print();
    }
    
    // -0.2347x^3 + 0.6338x^2 - 0.0329x + 0.9873
    // 0.9096x^3 - 3.8292x^2 + 5.7691x - 1.5268
    // -2.2594x^3 + 14.2342x^2 - 28.5513x + 20.2094
}
```

### MATLAB like macro

* `zeros` - zero vector or matrix
* `eye` - identity matrix
* `rand` - random matrix (range from 0 to 1)

### Automatic Differentiation

* Implemented AD with dual number structure.
* Available functions
    * `sin, cos, tan`
    * `pow, powf`
    * `+,-,x,/`
    * `exp, ln`

```rust
extern crate peroxide;
use peroxide::*;

fn main() {
    let a = dual(0, 1); // x at x = 0
    a.sin().print();    // sin(x) at x = 0
    
    // value: 0  // sin(0) = 0
    // slope: 1  // cos(0) = 1
}
```

### Jacobian

* Implemented by AD - Exact Jacobian

```rust
extern crate peroxide;
use peroxide::*;

fn main() {
    let xs = c!(1, 1);
    jacobian(xs, f).print();
    
    //      c[0] c[1]
    // r[0]    6    3
}

// f(t, x) = 3t^2 * x
fn f(xs: Vec<Dual>) -> Vec<Dual> {
    let t = xs[0];
    let x = xs[1];

    vec![t.pow(2) * 3. * x]
}
```

### Ordinary Differential Equation

* Solve 1st order ODE with various methods
* Explicit Method
    * `RK4`: Runge-Kutta 4th order
* Implicit Method
    * `BDF1`: Backward Euler
    * `GL4`: Gauss-Legendre 4th order
    
**Caution**

* input function should have form `(Dual, Vec<Dual>) -> Vec<Dual>`    
    
```rust
// Lotka-Volterra
extern crate peroxide;
use peroxide::*;

fn main() {
    // t = 0, x = 2, y = 1
    let xs = c!(2, 1);
    let rk4_records = solve(lotka_volterra, xs.clone(), (0, 10), 1e-3, RK4);
    let bdf_records = solve(lotka_volterra, xs.clone(), (0, 10), 1e-3, BDF1(1e-15));
    let gl4_records = solve(lotka_volterra, xs, (0, 10), 1e-3, GL4(1e-15));
    //rk4_records.write_with_header("example_data/lotka_rk4.csv", vec!["t", "x", "y"]);
    //bdf_records.write_with_header("example_data/lotka_bdf.csv", vec!["t", "x", "y"]);
    gl4_records.write_with_header("example_data/lotka_gl4.csv", vec!["t", "x", "y"]);
}

fn lotka_volterra(_t: Dual, xs: Vec<Dual>) -> Vec<Dual> {
    let a = 4.;
    let c = 1.;

    let x = xs[0];
    let y = xs[1];

    vec![
        a * (x - x*y),
        -c * (y - x*y)
    ]
}
```

## Version Info

To see [RELEASES.md](./RELEASES.md)

## TODO

To see [TODO.md](./TODO.md)