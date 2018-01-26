== Development notes for typeclass resolution caching ==

To cache:
1. Goal.t
2. Evars
3. autoinfo

=== Concerns: ===
* If we cache all failed searches, cache search size could become quite large. To be tested in real life with simple placeholder implementation/

=== Discussion with Matthieu at POPL'18: ===

* Use `Progress.goal_equals` instead `Goal.V82.same_goal`
* Use hints from autoinfo
* Hint_db.t equality but OK to start with =
* autoinfo: ignore depth, last_tac, search_cut
* Do not cache failures due to hint cut.
* `hints_tac` invokes monad
* How to pass cache:
  1. put cache into autoinfo (need to modify for it to fold (return back)
  2. Or global mutable ref (access via NonLogical monad)

=== Discussion with Arnaud Spiwack at POPL'18: ===
* Use tclLIFT/NonLogical to isolate I/O (cache access)

=== TODO ===
* Persistently save and load the cache
* Do not cache failures due to hint cut.
* Debug command to examine the cache?

=== Problems ===

In `tc_cache_entry_cmp` call to `Evarutil.eq_constr_univs_test` causes exception: `Anomaly: Universe Foo.N undefined.` One possible problem is that `eq_constr_univs_test` defined that "The universe constraints in [sigma2] are assumed to be an extention of those in [sigma1]". In caching this is not the case. In such situation comparison should always fail without excepion.







