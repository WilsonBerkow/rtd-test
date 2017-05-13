# java-education

Experimentation with tools that could be useful for teaching newbies.

*TODO:* update `guiExamples` to match changes in the design. See
`java-edu-2017` for example code.

### Packages

- `gui` &mdash; A library on top of `javafx` with simple interfaces for
getting keyboard and mouse input and drawing shapes to a canvas.

- `ask` &mdash; Very simple bindings for command-line IO. No `Scanner`
objects and the like.

- `guiExamples`, `askExamples` &mdash; Example programs using `gui` or `ask`,
which could be written by or with the newbies during newbie education.

### Building and running


#### Ant:

To compile:

```
$ ant compile
```

To run:

```
$ ant run -Dclass=<CLASS>
```

E.g.:

```
$ ant compile
$ ant run -Dclass=guiExamples.bouncing.BouncingXY
```

To generate JARs, first compile, and then run one of the following:

```
$ ant dist-ask # Creates dist/javaEducationAsk.jar
$ ant dist-gui # Creates dist/javaEducationGui.jar
$ ant dist # Creates both
```

#### Make:

To compile:

```
$ make
```

To run:

```
$ make run class=<CLASS>
```

E.g.:

```
$ make
$ make run class=guiExamples.bouncing.BouncingXY
```

To generate JARs, first compile, and then run the following:

```
$ make dist # Creates dist/Ask.jar and dist/Gui.jar
```

---

#### What's the `LICENSE` about?

It's called the [MIT License](http://choosealicense.com/licenses/mit/).
We'd love for other people and teams to use this project, and the MIT License
makes sure you can.

