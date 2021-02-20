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
