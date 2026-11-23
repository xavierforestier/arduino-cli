# arduino-cli
Dependencies for gentoo overlay

Copy previous version to the new one and remove packages 
```bash
sed -E 's/^(.*-vendor.tar.xz")$/" #\1/g' "$( find . | grep '\.ebuild$' | sort -rV | head -n1 )" > arduino-cli-x.y.z.ebuild
```
Create tarball
```bash
ebuild "$( find . | grep '\.ebuild' | sort -rV | head -n1 )" digest clean unpack || true
pushd "/var/tmp/portage/$( pwd | rev | cut -d/ -f2 | rev )/$( find . | grep '\.ebuild$' | sort -rV | head -n1 | cut -d/ -f2- | rev | cut -d. -f2- | rev )/work/$( find . | grep '\.ebuild$' | sort -rV | head -n1 | cut -d/ -f2- | rev | cut -d. -f2- | rev )"
go mod vendor
cd ..
XZ_OPT=-9 tar -Jcvf $( pwd | rev | cut -d/ -f2 | rev )-vendor.tar.xz $( pwd | rev | cut -d/ -f2 | rev )/vendor
```
Upload tarball as a github new release,
then finish ebuild
```bash
popd
sed -E 's/^" #(.*-vendor.tar.xz")$/\1/g' -i "$( find . | grep '\.ebuild$' | sort -rV | head -n1 )"
ebuild "$( find . | grep '\.ebuild' | sort -rV | head -n1 )" digest clean install
```
