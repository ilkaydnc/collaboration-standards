### <center>Ortak Çalışma Standardizasyonu</center>
>_Bu yazı, farklı kaynaklardaki yazıların birleştirilmiş, Türkçeleştirilmiş ve özet niteliği taşıyan bir versiyonudur. Bahsedilen konular hakkında daha detaylı bilgiye yazı sonundaki kaynak bağlantılarından ulaşabilirsiniz._

## Git Flow
Projeyi sadece `master` branch'te geliştirmek yerine, `develop` ve `master` isimli 2 farklı branch kullanmak daha mantıklıdır. `master` branch'inde, projenin resmi olarak çıkarılan sürümleri yer alır. Versiyon numaralarıyla etiketlemek için uygun bir branch'tir. `develop` branch'inde ise `master` branch'inde bir sonraki sürümde yayınlanacak, projenin en güncel hali bulunur. Yeni `feature`'lar bu branch'te eklenir.
<p align="center">
  <img width="600" src="https://wac-cdn.atlassian.com/dam/jcr:2bef0bef-22bc-4485-94b9-a9422f70f11c/02%20(2).svg?cdnVersion=1393" />
</p>

### Feature Branches
Bu branch'te projeye eklenecek olan yeni bir özelliğin geliştirmesi yapılır. `develop` branch'inden ayrılır ve tekrar `develop` branch'ine merge edilir. `master` branch'iyle doğrudan bir bağlantısı olmamalıdır. `feature` branch'leri, `develop` branch'inin en güncel halinden oluşturulmalıdır.
<p align="center">
  <img width="600" src="https://wac-cdn.atlassian.com/dam/jcr:b5259cce-6245-49f2-b89b-9871f9ee3fa4/03%20(2).svg?cdnVersion=1393" />
</p>
