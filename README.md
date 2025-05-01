# flang-dumper

Dumper of flang's AST into a JSON format.

## Building

```sh
mkdir build
cd build
cmake ..
make
```

## Running

```sh
$ cat > test.f90
real function foo(x) result(y)
  real, intent(in) :: x
  y = 1.0 - 1.0 / (1.0 - x + 0.5 * x*x)
end function

real function bar(x) result(y)
  real, intent(in) :: x
  y = 1.0 - 2.0 / (2.0 - (2.0 - x) * x)
end function

subroutine baz(x, y)
  real, intent(in) :: x
  real, intent(out) :: y
  y = x * x
end subroutine
<Ctrl+D>
$ flang-new-19 -fc1 -load build/libplugin.so -plugin print-fns test.f90
<outputs>
Function:	foo
Function:	bar
Subroutine:	baz

====   Functions: 2 ====
==== Subroutines: 1 ====
```

## Dependencies

**Node.js is required to build this project**

```sh
# Required for all targets
sudo apt install flang-19 libflang-19-dev llvm-19-dev libmlir-19-dev mlir-19-tools
```
