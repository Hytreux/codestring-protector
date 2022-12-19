# codestring-protector
Obfuscate
C++ 14 için garantili derleme zamanı dizesi hazır bilgi gizleme yalnızca üst bilgi kitaplığı.

Hızlı başlangıç ​​Kılavuzu
obfuscate.h dosyasını projenize kopyalayın
Dizeleri AY_OBFUSCATE("My String") ile koruyun
Artık projeniz bu dizeleri ikili görüntüde düz metin olarak göstermeyecek.

Bu dizilere, kararlı bilgisayar korsanları tarafından erişilebilir olmaya devam edeceğini unutmayın. Yazar, özel parolaları veya diğer güvenlik açısından hassas dizeleri gizlemek için karartma kullanılması önerilmez.

Sorun ne?
Düz metin dize sabit değerleri C++ programlarında kullanıldığında, olduğu gibi sonuç ikili dosyasında derlenir. Bu, bulunmalarının inanılmaz derecede kolay olmasına neden olur. Tüm katıştırılmış dize sabit değerlerini düz görünümde görmek için ikili dosyayı bir metin düzenleyicide açabilirsiniz. İkili dosyalarda düz metin dizeleri aramak için kullanılabilen, dizeler adı verilen özel bir yardımcı program mevcuttur.

Bu kütüphane ne yapıyor?
Bu salt başlık kitaplığı, derleyiciyi şifrelenmiş dizeyle çalışmaya zorlayarak derleme zamanında sabit bir ifade kullanarak bir XOR şifresi ile şifreleyerek ikili dosyalardaki katıştırılmış dize sabit değerlerinin bulunmasını zorlaştırır (ancak imkansız değildir). düz metin değişmezi yerine. AY_OBFUSCATE kullanımı ek olarak dizge için bir const işaretçisine olan ihtiyacı ortadan kaldırır; bu, çoğu zaman (küçük diziler için) derleyiciyi şifrelenmiş dizgiyi satır içine almaya ikna eder, onu çalışma zamanında bir dizi derleme işleminde oluşturur ve ikili görüntüyü korur. basit XOR şifre çözme saldırılarına karşı. Şifrelenmiş dizgilerin şifresi daha sonra program içinde kullanılmak üzere çalışma zamanında çözülecektir.

Teknik özellikler
Garantili derleme zamanı şaşırtması - dize, bir constexpr ifadesiyle derlenir.
Küresel ömür (iş parçacığı başına) - karartılmış dize, benzersiz bir lambda içinde bir iş parçacığı yerel değişkeninde depolanır.
Örtülü olarak bir karaktere dönüştürülebilir* - mevcut kod tabanlarına kolayca entegre edilebilir.
Rastgele 64 bitlik anahtar - her seferinde rastgele bir anahtarla karıştırılır.
"My String" dizginizi AY_OBFUSCATE("My String") ile sardığınızda, derleme zamanında rasgele bir 64 bitlik anahtarla şifrelenecek ve çalışma zamanında manipüle edebileceğiniz bir ay::obfuscated_data nesnesinde saklanacaktır. Kolaylık sağlamak için dolaylı olarak bir karaktere* dönüştürülebilir.

Örneğin, aşağıdaki program "Merhaba Dünya" dizesini derlenmiş yürütülebilir dosyanın hiçbir yerinde düz metin olarak saklamaz.

#include "obf"

#include "obfuscate.h"

int main()
{
    std::cout << AY_OBFUSCATE("Hello World") << std::endl;
    return 0;
}
