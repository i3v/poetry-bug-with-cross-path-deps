# Bug with cross path dependencies

```console
$ poetry --version
Poetry version 1.1.0

$ poetry lock -vvv
Creating virtualenv poetry-bug-with-cross-editable-deps-XDr2e7X_-py3.8 in /Users/albert/Library/Caches/pypoetry/virtualenvs
Using virtualenv: /Users/albert/Library/Caches/pypoetry/virtualenvs/poetry-bug-with-cross-editable-deps-XDr2e7X_-py3.8
Updating dependencies
Resolving dependencies...
   1: fact: poetry-bug-with-cross-editable-deps is 0.1.0
   1: derived: poetry-bug-with-cross-editable-deps
   1: fact: poetry-bug-with-cross-editable-deps depends on package1 (0.1.0 deps/package1)
   1: fact: poetry-bug-with-cross-editable-deps depends on package2 (0.2.0 deps/package2)
   1: selecting poetry-bug-with-cross-editable-deps (0.1.0)
   1: derived: package2 (0.2.0 deps/package2)
   1: derived: package1 (0.1.0 deps/package1)
   1: selecting package2 (0.2.0 /Users/albert/Projects/poetry-bug-with-cross-editable-deps/deps/package2)
   1: fact: package1 (0.1.0 /Users/albert/Projects/poetry-bug-with-cross-editable-deps/deps/package1) depends on package2 (0.2.0 deps/package2)
   1: conflict: package1 (0.1.0 /Users/albert/Projects/poetry-bug-with-cross-editable-deps/deps/package1) depends on package2 (0.2.0 deps/package2)
   1: ! package1 (0.1.0 /Users/albert/Projects/poetry-bug-with-cross-editable-deps/deps/package1) is satisfied by package1 (0.1.0 deps/package1)
   1: ! which is caused by "poetry-bug-with-cross-editable-deps depends on package1 (0.1.0 deps/package1)"
   1: ! thus: package2 is required
   1: ! not package2 (0.2.0 deps/package2) is satisfied by package2 (0.2.0 deps/package2)
   1: ! which is caused by "poetry-bug-with-cross-editable-deps depends on package2 (0.2.0 deps/package2)"
   1: ! thus: version solving failed
   1: Version solving took 0.051 seconds.
   1: Tried 1 solutions.

  Stack trace:

  8  ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/clikit/console_application.py:131 in run
      129│             parsed_args = resolved_command.args
      130│
    → 131│             status_code = command.handle(parsed_args, io)
      132│         except KeyboardInterrupt:
      133│             status_code = 1

  7  ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/clikit/api/command/command.py:120 in handle
      118│     def handle(self, args, io):  # type: (Args, IO) -> int
      119│         try:
    → 120│             status_code = self._do_handle(args, io)
      121│         except KeyboardInterrupt:
      122│             if io.is_debug():

  6  ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/clikit/api/command/command.py:171 in _do_handle
      169│         handler_method = self._config.handler_method
      170│
    → 171│         return getattr(handler, handler_method)(args, io, self)
      172│
      173│     def __repr__(self):  # type: () -> str

  5  ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/cleo/commands/command.py:92 in wrap_handle
       90│         self._command = command
       91│
    →  92│         return self.handle()
       93│
       94│     def handle(self):  # type: () -> Optional[int]

  4  ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/poetry/console/commands/lock.py:26 in handle
      24│         self._installer.lock()
      25│
    → 26│         return self._installer.run()
      27│

  3  ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/poetry/installation/installer.py:99 in run
       97│         local_repo = Repository()
       98│
    →  99│         return self._do_install(local_repo)
      100│
      101│     def dry_run(self, dry_run=True):  # type: (bool) -> Installer

  2  ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/poetry/installation/installer.py:205 in _do_install
      203│             )
      204│
    → 205│             ops = solver.solve(use_latest=self._whitelist)
      206│         else:
      207│             self._io.write_line("Installing dependencies from lock file")

  1  ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/poetry/puzzle/solver.py:65 in solve
       63│         with self._provider.progress():
       64│             start = time.time()
    →  65│             packages, depths = self._solve(use_latest=use_latest)
       66│             end = time.time()
       67│

  SolverProblemError

  Because poetry-bug-with-cross-editable-deps depends on package1 (0.1.0 deps/package1) which depends on package2 (0.2.0 deps/package2), package2 is required.
  So, because poetry-bug-with-cross-editable-deps depends on package2 (0.2.0 deps/package2), version solving failed.

  at ~/.pyenv/versions/3.8.0/lib/python3.8/site-packages/poetry/puzzle/solver.py:241 in _solve
      237│             packages = result.packages
      238│         except OverrideNeeded as e:
      239│             return self.solve_in_compatibility_mode(e.overrides, use_latest=use_latest)
      240│         except SolveFailure as e:
    → 241│             raise SolverProblemError(e)
      242│
      243│         results = dict(
      244│             depth_first_search(
      245│                 PackageNode(self._package, packages), aggregate_package_nodes
```

