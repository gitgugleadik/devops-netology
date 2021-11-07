# devops-netology
python add

проигнорировать все файлы в папках .terraform которые могут находится в других папках

проигнорировать все файлы с расширением tfstate
проигнорировать все файлы с любым расширением, которые в содержат в имени .tfstate.
проигнорировать файл crash.log
проигнорирова файлы с расширением tfvars

игнорировать файлы

override.tf
override.tf.json


и файлы которые оканчиваются на
_override.tf
_override.tf.json

игнорировать файлы
.terraformrc
terraform.rc

1
commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Jun 18 10:29:58 2020 -0400

    Update CHANGELOG.md

2
commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)
Author: tf-release-bot <terraform@hashicorp.com>
Date:   Thu Mar 5 20:56:10 2020 +0000

    v0.12.23

3
$ git show b8d720^
commit 56cd7859e05c36c06b56d013b55a252d0bb7e158
Merge: 58dcac4b7 ffbcf5581
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Mon Jan 13 13:19:09 2020 -0800

    Merge pull request #23857 from hashicorp/cgriggs01-stable

    [cherry-pick]add checkpoint links

2 родителя

$ git show 58dcac4b7
commit 58dcac4b798f0a2421170d84e507a42838101648 (tag: v0.12.19)
Author: tf-release-bot <terraform@hashicorp.com>
Date:   Wed Jan 8 22:38:48 2020 +0000

    v0.12.19

$ git show ffbcf5581
commit ffbcf55817cb96f6d5ffe1a34abe963b159bf34e
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Mon Jan 13 13:13:34 2020 -0800


4
commit 225466bc3e5f35baa5d07197bbc079345b77525e
Cleanup after v0.12.23 release

commit dd01a35078f040ca984cdd349f18d0b67e486c35
Update CHANGELOG.md

commit 4b6d06cc5dcb78af637bbb19c198faff37a066ed
Update CHANGELOG.md

commit d5f9411f5108260320064349b757f55c09bc4b80
command: Fix bug when using terraform login on Windows

commit 06275647e2b53d97d4f0a19a0fec11f6d69820b5
Update CHANGELOG.md

commit 5c619ca1baf2e21a155fcdb4c264cc9e24a2a353
website: Remove links to the getting started guide's old location

Since these links were in the soon-to-be-deprecated 0.11 language section, I
think we can just remove them without needing to find an equivalent link.

commit 6ae64e247b332925b872447e9ce869657281c2bf
    registry: Fix panic when server is unreachable

    Non-HTTP errors previously resulted in a panic due to dereferencing the
    resp pointer while it was nil, as part of rendering the error message.
    This commit changes the error message formatting to cope with a nil
    response, and extends test coverage.

    Fixes #24384

commit 3f235065b9347a758efadc92295b540ee0a5e26e
Update CHANGELOG.md


commit b14b74c4939dcab573326f4e3ee2a62e23e12f89
[Website] vmc provider links

commit 33ff1c03bb960b332be3af2e333462dde88b279e (tag: v0.12.24)
v0.12.24



5

$ git grep --count 'func providerSource'
provider_source.go:2


git log -L :providerSource:provider_source.go
commit 5af1e6234ab6da412fb8637393c5a17a1b293663
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Tue Apr 21 16:28:59 2020 -0700

    main: Honor explicit provider_installation CLI config when present

    If the CLI configuration contains a provider_installation block then we'll
    use the source configuration it describes instead of the implied one we'd
    build otherwise.


6


$ git grep --count 'func globalPluginDirs'
plugins.go:1


$ git log -L :'func globalPluginDirs':plugins.go
commit 78b12205587fe839f10d946ea3fdc06719decb05
commit 52dbf94834cb970b510f2fba853a5b49ad9b1a46
commit 41ab0aef7a0fe030e84018973a64135b11abcd70
commit 66ebff90cdfaa6938f26f908c7ebad8d547fea17
commit 8364383c359a6b738a436d1b7745ccdce178df47

7
Кто автор функции synchronizedWriters?
$ git grep --count 'synchronizedWriters'

$ git branch -a -v -r | git grep --count 'synchronizedWriters'
Такие запросы ничего не дали.
Такой функции нет.









