1. aefead2207ef7e2aa5dc81a34aedf0cad4c32545 Update CHANGELOG.md (найден через команду git show aefea)

2. v0.12.23 (найден через команду git show 85024d3);

3. результат можно получить несколькими способами, например, просто указав git show b8d720, мы увидим, что коммит был получен путём мержа. В выводе мы увидим два коротких хэша в поле Merge: 56cd7859e 9ea88f22f, которые и являются нашими родителями. 
Но нам нужен полный хэш, поэтому можно воспользоваться командой git log или git show с опцией --pretty, в которой указать, что нам нужен полный хэш коммитов (%P), в итоге, при вводе команды git show --pretty=%P b8d720 (git log --pretty=%P -1 b8d720) получаем родителей коммита b8d720:
56cd7859e05c36c06b56d013b55a252d0bb7e158 
9ea88f22fc6269854151c571162c5bcf958bee2b

Но более красивый вывод через git show --pretty=raw b8d720 (git log --pretty=raw -1 b8d720)
parent 56cd7859e05c36c06b56d013b55a252d0bb7e158
parent 9ea88f22fc6269854151c571162c5bcf958bee2b

4. с помощью команды git log --oneline --pretty=format:"%H %s" v0.12.23..v0.12.24 (можно использовать команду покороче, git log --pretty=oneline v0.12.23..v0.12.24) получаем список коммитов с комментарии:
(33ff1c03bb960b332be3af2e333462dde88b279e (tag: v0.12.24) v0.12.24
b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links
3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md
6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable
5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location
06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md
d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows
4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md
dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md
225466bc3e5f35baa5d07197bbc079345b77525e Cleanup after v0.12.23 release)

5. находим все коммиты, в которых была добавлена или удалена строка:
git log -S "func providerSource"
Результат выведет 2 коммита. Коммит, в котором была определена функция func providerSource (если просмотреть коммит с помощью команды git show 8c928e, то увидим определение функции в файле provider_source.go): 
8c928e83589d90a031f811fae52a81be7153e82f

6. находим все коммиты, в которых была добавлена или удалена строка:
git log -S "globalPluginDirs" --oneline
Получаем список коммитов:
35a058fb3
c0b176109
8364383c3
Далее смотрим самый ранний коммит:
git show 8364383c3
И видим, что искомая функция была добавлена в файле plugins.go. И теперь ищем изменения в функции globalPluginDirs в файле plugins.go:
git log -L :globalPluginDirs:plugins.go
Получаем список коммитов, в которых были изменения искомой функции:
commit 78b12205587fe839f10d946ea3fdc06719decb05
commit 52dbf94834cb970b510f2fba853a5b49ad9b1a46
commit 41ab0aef7a0fe030e84018973a64135b11abcd70
commit 66ebff90cdfaa6938f26f908c7ebad8d547fea17
commit 8364383c359a6b738a436d1b7745ccdce178df47
Можно было немного сократить поиск, воспользовавшись git grep "globalPluginDirs", который вывел бы нам информацию о всех файлах, где встречается функция, в том числе и файле, в котором определена функция func globalPluginDirs(), а дальше уже через флаг -L


7. git log -S "synchronizedWriters"
Author: Martin Atkins <mart@degeneration.co.uk>
