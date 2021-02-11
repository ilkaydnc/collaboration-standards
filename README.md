<h1 align="center">Ortak Çalışma Standardizasyonu</h1>

>_Bu yazı, farklı kaynaklardaki yazıların birleştirilmiş, Türkçeleştirilmiş ve özet niteliği taşıyan bir versiyonudur. Bahsedilen konular hakkında daha detaylı bilgiye yazı sonundaki kaynak bağlantılarından ulaşabilirsiniz._

## Git Flow
Projeyi sadece `master` branch'te geliştirmek yerine, `develop` ve `master` isimli 2 farklı branch kullanmak daha mantıklıdır. `master` branch'inde, projenin resmi olarak çıkarılan sürümleri yer alır. Versiyon numaralarıyla etiketlemek için uygun bir branch'tir. `develop` branch'inde ise `master` branch'inde bir sonraki sürümde yayınlanacak, projenin en güncel hali bulunur. Yeni özellikler bu branch'te eklenir.
<p align="center">
  <img width="600" src="https://wac-cdn.atlassian.com/dam/jcr:2bef0bef-22bc-4485-94b9-a9422f70f11c/02%20(2).svg?cdnVersion=1393" />
</p>

## Feature Branch'leri
Bu branch'te projeye eklenecek olan yeni bir özelliğin geliştirmesi yapılır. `develop` branch'inden ayrılır ve tekrar `develop` branch'ine merge edilir. `master` branch'iyle doğrudan bir bağlantısı olmamalıdır. `feature` branch'leri, `develop` branch'inin en güncel halinden oluşturulmalıdır.
<p align="center">
  <img width="600" src="https://wac-cdn.atlassian.com/dam/jcr:b5259cce-6245-49f2-b89b-9871f9ee3fa4/03%20(2).svg?cdnVersion=1393" />
</p>

### Yeni bir feature branch'i oluşturmak
`git-flow` eklentisi olmadan:
```bash
	git checkout develop  
	git checkout -b feature_branch
```
`git-flow` eklentisi kullanarak:
```bash
	git flow feature start feature_branch
```

### feature branch'ini tamamlamak
`git-flow` eklentisi olmadan:
```bash
	git checkout develop  
	git merge feature_branch
```
`git-flow` eklentisi kullanarak:
```bash
	git flow feature finish feature_branch
```

## Release Branch'leri
`develop` branch'imiz yeteri kadar özelliğe sahip olduğunda veya önceden belirlenmiş bir yayın tarihi varsa `develop` branch'inden bir `release` branch'i ayırmak gerekir. Bu branch'in oluşturulması bir sonraki sürüm döngüsünü başlatır. Bu noktadan sonra bu branch'e artık yeni bir özellik eklenemez. Yalnızca bug-fix, dökümanyasyon oluşturma veya bu sürümle alakalı işler eklenebilir. `release` branch'i hazır olduğunda `master` branch'ine gönderilir. Ek olarak `release` içerisindeki son değişikler tekrar'dan `develop` branch'ine gönderilmelidir.

`release`'ler için ayrı bir branch kullanmak, bir takım `release` branch'inde düzenlemeler yaparken diğer takımların bir sonraki sürüm için yeni özellikler geliştirebilmesine olanak sağlar.

<p align="center">
  <img width="600" src="https://wac-cdn.atlassian.com/dam/jcr:a9cea7b7-23c3-41a7-a4e0-affa053d9ea7/04%20(1).svg?cdnVersion=1449" />
</p>

### Yeni bir release branch'i oluşturmak
`git-flow` eklentisi olmadan:
```bash
	git checkout develop  
	git checkout -b release/0.1.0
```
`git-flow` eklentisi kullanarak:
```bash
	git flow release start 0.1.0
	# Output: "Switched to  a  new branch 'release/0.1.0'"
```
### release branch'ini tamamlamak
```bash
	git checkout master  
	git flow release finish '0.1.0'
```
`git-flow` eklentisi kullanarak:
```bash
	git flow release finish '0.1.0'
```

## Hotfix Branch'leri
Bakım veya `hotfix` branch'leri yayındaki bir sürümdeki hataları hızlıca gidermek amacıyla oluşturulur. `develop` branch'inden ayrılması dışında `release` branch'lerine oldukça benzerdir. `master` branch'inden fork'lanılması gereken tek branch türüdür. Hata çözüldüğü anda `master` ve `develop` branch'lerine gönderilmelidir ve `master` güncellenmiş sürüm numarası ile etiketlenmelidir.

`hotfix`'ler için ayrı branch kullanmak; takımınızın, sorunları iş akışını kesintiye uğratmadan veya bir sonraki sürümü beklemeden ele almasına olanak sağlar.

<p align="center">
  <img width="600" src="https://wac-cdn.atlassian.com/dam/jcr:61ccc620-5249-4338-be66-94d563f2843c/05%20(2).svg?cdnVersion=1449" />
</p>

### Yeni bir hotfix branch'i oluşturmak
`git-flow` eklentisi olmadan:
```bash
	git checkout master
	git checkout -b hotfix_branch
```
`git-flow` eklentisi kullanarak:
```bash
	git flow hotfix start hotfix_branch
```
### hotfix branch'ini tamamlamak
`git-flow` eklentisi olmadan:
```bash
	git checkout master
	git merge hotfix_branch
	
	git checkout develop
	git merge hotfix_branch
	
	git branch -D hotfix_branch
```
`git-flow` eklentisi kullanarak:
```bash
	git flow hotfix finish hotfix_branch
```

## Git Commit'leri Nasıl Yazılmalıdır?
### Giriş: İyi commit mesajları neden önemlidir?
Eğer rastgele bir Git repo'suna girdiğinizde büyük ihtimalle aşağıdaki gibi bir karmaşa göreceksiniz. Örnek olarak, [Spring Framework](https://github.com/spring-projects/spring-framework/commits/e5f4b49?author=cbeams)'ünün rastgele bir zamanındaki `commit`'lerine göz atalım.

```bash
$ git log --oneline -5 --author cbeams --before "Fri Mar 26 2009"

e5f4b49 Re-adding ConfigurationPostProcessorTests after its brief removal in r814. @Ignore-ing the testCglibClassesAreLoadedJustInTimeForEnhancement() method as it turns out this was one of the culprits in the recent build breakage. The classloader hacking causes subtle downstream effects, breaking unrelated tests. The test method is still useful, but should only be run on a manual basis to ensure CGLIB is not prematurely classloaded, and should not be run as part of the automated build.
2db0f12 fixed two build-breaking issues: + reverted ClassMetadataReadingVisitor to revision 794 + eliminated ConfigurationPostProcessorTests until further investigation determines why it causes downstream tests to fail (such as the seemingly unrelated ClassPathXmlApplicationContextTests)
147709f Tweaks to package-info.java files
22b25e0 Consolidated Util and MutableAnnotationUtils classes into existing AsmUtils
7f96f57 polishing
```
🙄🙄🙄 Yeterince kötü. Bir de aşağıdaki `commit`'lerle karşılaştırma yapalım.
```bash
$ git log --oneline -5 --author pwebb --before "Sat Aug 30 2014"

5ba3db6 Fix failing CompositePropertySourceTests
84564a0 Rework @PropertySource early parsing logic
e142fd1 Add tests for ImportSelector meta-data
887815f Update docbook dependency and generate epub
ac8326d Polish mockito usage
```
Çoğu repo'lar ilk kısımdaki gibi karmaşıktır. Ama [Linux Kernel](https://github.com/torvalds/linux/commits/master) ve [Git](https://github.com/git/git/commits/master) gibi örnek olabilecek güzel istisnalar bulunmaktadır.

Bu repo'lardaki `contributor`'lar, iyi yazılmış `commit`'lerin, bir değişiklikle ilgili bağlamın diğer geliştiricilere (ya da gelecekte kendilerine) iletmenin en iyi yolu olduğunu bilirler. `diff` size değişikliğin _ne_ olduğunu anlatır. Ama size o değişikliğin _neden_ yapıldığını sadece `commit`'ler anlatır. Peter Hutterer [yazısında](http://who-t.blogspot.com/2009/12/on-commit-messages.html) bu noktaya şöyle değinir:

> Bir kod parçasının ne işe yaradığını yeniden çözmeye çalışmak zaman israfıdır. Bundan tamamen kaçınamayız, bu yüzden bunu olabildiğinde azaltmak için çaba göstermeliyiz. `commit` mesajları tam olarak bu işe yarar ve bunun sonucu olarak; **bir `commit` mesajı, bir geliştiricinin iyi bir `contributor` olup olmadığını gösterir.**

Eğer iyi bir Git `commit`'inin neyin oluşturduğu hakkında fazla düşünmediyseniz, `git log` veya ilgili araçlar ile fazla vakit harcamamış olabilirsiniz. Burada bir kısır döngü var: `commit` geçmişi belli bir yapıdan uzak ve tutarsız olduğunda, onun üzerinde zaman harcamaz ve umursamaz. Ve üzerinde zaman harcanmadığında ve umursanmadığında, belli bir yapıdan uzak ve tutarsız bir geçmiş oluşur.

Ama iyi bakılmış bir geçmiş güzel ve faydalı bir şeydir. `git blame`, `revert`, `rebase`, `log`, `shortlog` ve buna benzer komutları hayatımıza sokar. Başkalarının `commit`'lerini veya `pull request`'lerini `review` yapmaya değer ve bağımsız halde yapmaya değer hale gelir.

### Harika bir Git `commit`'inin 7 kuralı
1.  [Konu satırını ve açıklama kısmını boş bir satır ile ayırmak](#separate)
2.  [Konu satırını 50 karakter ile sınırlandırmak](#limit-50)
3.  [Konu satırına büyük harf ile başlamak](#capitalize)
4.  [Konu satırını nokta ile bitirmemek](#end)
5.  [Konu satırında `imperative mood` kullanmak](#imperative)
6.  [İçerik kısmında her satırı 72 karakter ile sınırlandırmak](#wrap-72)
7.  [_Ne_, _neden_ ve _nasıl_ gibi soruları açıklama kısmında açıklamak](#why-not-how)

Örnek olarak:
```
Summarize changes in around 50 characters or less

Daha fazla açıklama içeren uzun metin, eğer gerekiyorsa. Her satır
72 karakterden az. Bazı içeriklerde, konu satırının devamı olacak 
şekilde olabilir. İçeriği konu satırından ayıran boş satır kritiktir.

Commit'in çözdüğü sorunu, sorunu neden bu şekilde
çözdüğünüzü, bunun ne gibi şeyleri etkilediğini ve ne gibi sonuçlar
doğurabileceğini burada açıklayın.

Boş satırlardan sonra yeni paragraflar gelir.

 - Maddeler kullanılabilir.

 - Maddelerin öncesinde tire veya yıldız kullanılmalıdır ve her maddeden
 önce bir satır boşluk bırakılmalıdır.

Eğer bir `issue tracker` kullanıyorsanız, burada alakalı madde 
numaralarını yazabilirsiniz.

Resolves: #123
See also: #456, #789
```

### 1. Konu satırını ve açıklama kısmını boş bir satır ile ayırmak

`git commit` [kılavuz](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-commit.html#_discussion) sayfasından:

>Gerekli olmasa da, `commit` mesajına değişikliği özetleyen tek bir kısa satır (50 karakterden az), ardından boş bir satır ve ardından daha kapsamlı bir açıklama ile başlamak iyi bir fikirdir. Bir `commit` mesajında ilk boş satıra kadar olan metin `commit` başlığı olarak kabul edilir ve bu başlık Git'te kullanılır.

Öncelikle, her `commit` açıklamaya ihtiyaç duymayabilir. Bazen tek bir satır yeterlidir. Örneğin:
```
Fix typo in introduction to user guide
```

Git geçmişini okuyan birisi yazım hatasının nerede olduğunu merak ederse, değişimin ne olduğuna kolaylıklı bakabilir. Örnek olarak: `git show` veya `git diff` veya `git log -p`.

Bununla birlikte, bir `commit`'in açıklamaya ihtiyacı olduğunda `-m` seçeneğiyle yazmak o kadar kolay değildir. Bu noktada Git GUI kullanmak mantıklıdır. Komut satırına `git gui` yazarak Git ile beraber gelen GUI'yi kullanabilirsiniz veya üçüncü parti yazılımlar kullanmak da mümkündür.

```
Derezz the master control program

MCP turned out to be evil and had become intent on world domination.
This commit throws Tron's disc into MCP (causing its deresolution)
and turns it back into a chess game.
```

Bu durumda konu satırının açıklamadan ayrılması, geçmişe göz atarken iş yarıyor. Tam geçmiş şu şekilde:
```
$ git log
commit 42e769bdf4894310333942ffc5a15151222a87be
Author: Kevin Flynn <kevin@flynnsarcade.com>
Date:   Fri Jan 01 00:00:00 1982 -0200

 Derezz the master control program

 MCP turned out to be evil and had become intent on world domination.
 This commit throws Tron's disc into MCP (causing its deresolution)
 and turns it back into a chess game.
```

### 2. Limit the subject line to 50 characters
50 karakter kesin bir sınır değildir, sadece bir kuraldır. Konu satırlarının bu uzunlukta tutulması, okunabilir olmalarını sağlar ve yazarı, neler olduğunu açıklamanın en kısa yolu hakkında bir an düşünmeye zorlar.

> Eğer 50 karakterden az yazma konusunda zorlanıyorsanız, aynı anda çok fazla değişiklik yapıyor olabilirsiniz. Atomic `commit`'ler konusunda çaba göstermeniz gerekir.

GitHub'ın UI'yı bu kuralların tamamen farkındadır ve 50 karakter sınırını aşarsanız sizi uyarır:

![enter image description here](https://i.imgur.com/zyBU2l6.png)

Ve 72 karakterden uzun olan konu satırlarını 3 nokta ile keser:

![enter image description here](https://i.imgur.com/27n9O8y.png)
Sonuç olarak, 50 karakter sınırı için çabalayın ama 72 karakter sınırını kesin olarak aşmamanız gerekir.

### 3. Konu satırına büyük harf ile başlamak
Başlıktan da anlaşılacaği gibi gayet bir maddedir. Tüm konu satırlarına büyük harf kullanarak başlayın.

Örnek olarak:
- Accelerate to 88 miles per hour

Bunun yerine yukarıdaki örnekteki gibi yazmak daha iyidir.
- ~~accelerate to 88 miles per hour~~

### 4. Konu satırını nokta ile bitirmemek
Konu satırlarının sonundaki noktalama işaretleri gereksizdir. Ayrıca, 50 karakter sınırını korumaya çalışırken her harf bizim için değerlidir.

Örnek olarak:
- Open the pod bay doors

Bunun yerine yukarıdaki örnekteki gibi yazmak daha iyidir.
- ~~Open the pod bay doors.~~

### 5. Konu satırında `imperative mood` kullanmak
`imperative mood`, sözlü veya yazılı bir komut veya talimat verme anlamına gelir. Birkaç örnek:
-   Clean your room
-   Close the door
-   Take out the trash

Bu yazıdaki 7 kuralın her biri `imperative mood` kullanarak yazılmıştır. Örnek olarak: "Wrap the body at 72 characters".

`imperative mood` kaba gelebilir. Bu yüzden bunu hayatımızda kullanmamaya dikkat ediyoruz. Ama Git `commit`'leri için mükemmeldir. Bunun sebebi de **Git'in kendisi, sizin için bir `commit` oluşturduğunda `imperative mood` kullanır.**

Örnek olarak, `git merge` kullanılırken oluşturulan varsayılan mesaj şu şekildedir:
```
Merge branch 'myfeature'
```
Ve `git revert` kullandığınızda:

```
Revert "Add the thing with the stuff"

This reverts commit cc87791524aedd593cff5a74532befe7ab69ce9d.
```
Veya GitHub pull request'inde "Merge" button'una tıkladığınızda:
```
Merge pull request #123 from someuser/somebranch
```
Bu nedenle `commit` mesajlarınızı `imperative mood`'da yazdığınızda, Git'in kendi yerleşik kurallarını uygulamış olursunuz.

-   Refactor subsystem X for readability
-   Update getting started documentation
-   Remove deprecated methods
-   Release version 1.0.0

Bu şekilde yazmak ilk başlarda biraz tuhaf gelebilir. `indicative mood`'ta konuşmaya daha alışkınız. Bu nedenle `commit` mesajlarını şu şekilde okunur:

- ~~Fixed bug with Y~~
- ~~Changing behavior of X~~

Ve bazen `commit`'ler içeriğinin açıklaması şeklinde yazılır:

- ~~More fixes for broken stuff~~
- ~~Sweet new API methods~~

Doğru yazıp yazmama karışıklığını engellemek için, her zaman işe yarayan aşağıdaki gibi bir kural vardır.

**Uygun şekilde oluşturulmuş bir `commit` mesajı her zaman aşağıdaki cümleyi tamamlayabilmelidir.**:

- If applied, this commit will  **_your subject line here_**

Örnek olarak:

-   If applied, this commit will **_refactor subsystem X for readability_**
-   If applied, this commit will **_update getting started documentation_**
-   If applied, this commit will **_remove deprecated methods_**
-   If applied, this commit will **_release version 1.0.0_**
-   If applied, this commit will **_merge pull request #123 from user/branch_**

`imperative mood` kullanılmadığı durumlarda yazılan konu satırlarının neden doğru olmadığına bakalım:

-   If applied, this commit will ~~**_fixed bug with Y_**~~
-   If applied, this commit will ~~**_changing behavior of X_**~~
-   If applied, this commit will ~~**_more fixes for broken stuff_**~~
-   If applied, this commit will ~~**_sweet new API methods_**~~

>`imperative mood` kullanımı sadece konu satırlarında önemlidir. Açıklama kısmını yazarken bu kısıtlamaya ihtiyaç olmaz.

### 6. İçerik kısmında her satırı 72 karakter ile sınırlandırmak
Git yazıları otomatik olarak yeni satıra geçirmez. Bir `commit` mesajının açıklamasını yazarken onun sağ kenar boşluğuna dikkat etmeli ve metni kendiniz alt satıra geçirmelisiniz.

### 7. _Ne_, _neden_ ve _nasıl_ gibi soruları açıklama kısmında açıklamak
[Bitcoin Core'un bu `commit`'i](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6), neyin neden değiştiğini açıklama konusunda mükemmel bir örnektir.
```
commit eb0b56b19017ab5c16c745e6da39c53126924ed6
Author: Pieter Wuille <pieter.wuille@gmail.com>
Date:   Fri Aug 1 22:57:55 2014 +0200

   Simplify serialize.h's exception handling

   Remove the 'state' and 'exceptmask' from serialize.h's stream
   implementations, as well as related methods.

   As exceptmask always included 'failbit', and setstate was always
   called with bits = failbit, all it did was immediately raise an
   exception. Get rid of those variables, and replace the setstate
   with direct exception throwing (which also removes some dead
   code).

   As a result, good() is never reached after a failure (there are
   only 2 calls, one of which is in tests), and can just be replaced
   by !eof().

   fail(), clear(n) and exceptions() are just never called. Delete
   them.
```
[Tüm değişikliklere](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6) göz atın ve `commit` yazarının bu değişikliği burada ve şimdi açıklayarak, gelecekteki `commit` atacak olanlara kazandırdığı zamanı düşünün.

Çoğu durumda, bir değişikliğin nasıl yapıldığına dair ayrıntıları atlayabilirsiniz. Kod bu bakımdan genellikle kendini açıklar niteliktedir. Eğer kod fazla karmaşıksa ve açıklanması gerekiyorsa, açıklama kısmı bunun içindir. En başta neden değişiklik yaptığınızı, yani değişiklikten önce işlerin nasıl yürüdüğünü (ve bunda neyin yanlış olduğunu), şimdi çalışma şeklini ve neden bu şekilde çözmeye karar verdiğinizi netleştirmeye odaklanın

## Kaynaklar
* **Git Flow** (Makale ve Görseller): [https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
* **Git Commit Standartları** (Makale): [https://chris.beams.io/posts/git-commit/](https://chris.beams.io/posts/git-commit/)
