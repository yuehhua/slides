
class: center, middle






# How to perform TDD and CI in Julia?






#### Speaker: Yueh-Hua Tu






##### 2021.1.14


---






## Test-driven development


<img src="../pics/TDD.png" width="70%" style="display: block;margin-left: auto; margin-right: auto;">


> Wikipedia: Test-driven development



---




## Test-driven development


  * Testing is to guide development


--


  * Need a efficient way to test


--


  * Refuel for example


```
sh> julia --proj
(Refuel) pkg> test
```


---


class: middle






## Start from generating a project


```julia
using PkgTemplates

t = Template(;
    user="yuehhua",
    dir="/media/yuehhua/Workbench/workspace",
    authors="Yueh-Hua Tu",
    julia=v"1.5",
    plugins=[
        License(; name="MIT"),
        Git(; manifest=true, ssh=true),
        GitHubActions(),
        Codecov(),
        Documenter{GitHubActions}(),
        Develop(),
        TagBot()
    ],
)

t("MyProject")
```


---


class: middle






## Project layout


```julia
.
├── docs
│   ├── make.jl
│   ├── Manifest.toml
│   ├── Project.toml
│   └── src
│       └── index.md
├── LICENSE
├── Manifest.toml
├── Project.toml
├── README.md
├── src
│   └── MyProject.jl
└── test
    └── runtests.jl

4 directories, 10 files
```


---


class: middle






## Write testing






##### test/runtests.jl


```julia
using MyProject
using Test

@testset "MyProject.jl" begin
    @test foo() == "Hello, world!"
end
```


---


class: middle






## Write your code






##### src/MyProject.jl


```julia
module MyProject

foo() = "Hello, world!"

end
```


---


class: middle






## Fail!?


```julia
MyProject.jl: Error During Test at /media/yuehhua/Workbench/workspace/MyProject/test/runtests.jl:5
  Test threw exception
  Expression: foo() == "Hello, world!"
  UndefVarError: foo not defined
  Stacktrace:
   [1] top-level scope at /media/yuehhua/Workbench/workspace/MyProject/test/runtests.jl:5
   [2] top-level scope at /build/julia/src/julia-1.5.3/usr/share/julia/stdlib/v1.5/Test/src/Test.jl:1115
   [3] top-level scope at /media/yuehhua/Workbench/workspace/MyProject/test/runtests.jl:5
```


--






#### Remember to export functions


```julia
module MyProject

export foo

foo() = "Hello, world!"

end
```


---


class: middle






## Congratulation! Test passed!


```julia
Test Summary: | Pass  Total
MyProject.jl  |    1      1
    Testing MyProject tests passed
```


---


class: middle






## Commit your code


  * If the development is sufficient for a commit, just submit your commit to repository.


--


  * If the submission triggers CI to start, check if CI is success.


---






## What's next?


Configure your CI server!






#### Free for open source


  * Travis CI
  * Circle CI
  * Github Action
  * Gitlab CI


--






#### Self-hosted CI


  * Jenkins
  * Gitlab CI


--






#### Let's check demo


---


class: middle






# Thank you for attention
