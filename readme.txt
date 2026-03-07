Copy or move handling same file name by incremental directory.

2 arguments (as cp & mv): source (file(s), folder(s)) & directory.
0≤N.

cpn or mvn:
Copy-paste source to directory
If source & directory/content (file(s), folder(s)) same name, copy-paste or move to directory/0 to create if exists not
If source & directory/N/content same name, copy-paste or move to directory/N+1 to create if exists not.

I asked to implement in cp & mv https://lists.gnu.org/archive/html/coreutils/2026-02/msg00030.html without success, ignored...

E.g.:
mvn /source/*.txt /dest
