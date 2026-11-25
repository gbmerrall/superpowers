# Building Without a Plan

You are the builder, the approved design doc is your spec, and the
native tasks you created during design are your work list. Skipping the
plan skips a document. The isolated workspace, the tests and the
fresh-context review stay.

1. **Workspace.** Use superpowers-extended-cc:using-git-worktrees for an isolated
   workspace. Never start on a main/master branch without your human
   partner's explicit consent. Record the commit the branch starts from;
   the review needs it.
2. **Tasks.** Work the native tasks in dependency order, each under
   superpowers-extended-cc:test-driven-development. Mark a task `in_progress` when
   you start it. Before you mark it `completed`, run its Verify and read
   the output, then commit. If this session carries a
   `<workflow-config-active>` notice setting `commitStrategy` to
   `at-end`, make no per-task commits: make one commit of the whole
   change set after the last task.
3. **Review.** When every task is done, get one fresh-context review of
   the whole branch through superpowers-extended-cc:requesting-code-review. The
   requirements are the design doc. The range is the whole branch: the
   starting commit you recorded (or `git merge-base <base-branch> HEAD`)
   to `HEAD`, never `HEAD~1`, which covers only the last commit. Fix the
   Critical and Important findings, each with a test that failed first.
4. **Finish.** Use superpowers-extended-cc:verification-before-completion, then
   superpowers-extended-cc:finishing-a-development-branch.

Your final message lists every decision you made that the design doc
left open, so your human partner can check them.

If the build grows until one of the plan's jobs becomes real (the work
will outlast your context, or you want to hand tasks to subagents),
stop, say so, and invoke superpowers-extended-cc:writing-plans for what remains.
