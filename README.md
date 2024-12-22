<!-- Project Title -->
<h1 align="center"> 42 - cub3d 🕹️</h1>

<!-- Project Logo or Image -->
<p align="center">
  <a target="blank"><img src="https://i.hizliresim.com/lc3txl2.png?_gl=1*1ia0uuv*_ga*MTg2MDIyNTgxMC4xNzM0ODc2OTYy*_ga_M9ZRXYS2YN*MTczNDg3Njk2MS4xLjEuMTczNDg3Njk3Ny40NC4wLjA." height="150" width="150" /></a>
</p>

<!-- Project Description -->
<p align="center">
Cub3d projesi, bir 3D labirent oyunu oluşturmayı amaçlar. Oyuncu, birinci şahıs bakış açısıyla bir labirentte gezinerek belirli hedeflere ulaşmaya çalışır. Bu proje, oyun programlamasının temel unsurlarını, grafik yönetimini ve kullanıcı etkileşimini kapsamaktadır. Ayrıca, temel 3D oyun mekaniği, çarpışma algılama ve olay yönetimi alanlarında deneyim kazandırır.
</p>

---

## Projenin Amacı 🎯

Cub3d projesinin amacı, birinci şahıs bakış açısıyla oynanan bir 3D labirent oyunu oluşturmaktır. Oyunda, kullanıcı bir karakteri labirent içinde yönlendirerek belirli hedeflere ulaşmayı amaçlar. Bu projede, 3D proje yapısı, oyun mekaniği, grafik yönetimi ve kullanıcı etkileşimi gibi konulara odaklanılır.

Labirent, bir dizi harita dosyasından oluşturulur ve karakter, oyuncunun yönlendirmesiyle bu ortamda hareket eder. Proje, çarpışma algılama, oyun olaylarını yönetme ve kullanıcı etkileşimlerini işleme gibi temel oyun programlama becerilerini geliştirmeye yöneliktir.

### Core Objectives 🏆

- **Maze Creation (3D):** 3D ortamda harita dosyalarını okuyup doğru şekilde ekrana yansıtmak. Harita dosyaları duvar, geçiş alanları ve hedef noktalarını içerecek şekilde oyunun görsel yapısını ve işlevselliğini belirler. 🗺️
- **Character Movement:** Kullanıcının, birinci şahıs bakış açısındaki karakteri ok tuşları veya benzeri kontrol tuşları ile özgürce hareket ettirebilmesi. Bu hareket, oyunun akıcılığı ve etkileşimi açısından önemlidir. 🕹️
- **Collision Detection:** Karakter ile duvarlar veya diğer nesneler arasındaki çarpışmaların algılanması. Böylece geçilemez alanlarla etkileşim yönetilir ve oyunun fizik kuralları uygulanır. 🚧
- **Game Events:** Oyunun başlangıcı, bitişi ve diğer kullanıcı etkileşimlerini yönetme. Bu, oyun akışını, oyuncu geri bildirimlerini, bölüm geçişlerini ve hedef tamamlama işlemlerini içerir. 🎮
- **User Interaction:** Kullanıcı arayüzünü ve kontrol mekanizmalarını optimize ederek, oyuncunun oyuna kolay adapte olmasını ve keyifli bir deneyim yaşamasını sağlamak. 🖱️
- **Game Performance:** Oyunun genel performansını ve verimliliğini artırmak. Akıcı grafik render işlemleri ve etkileşim yönetimi, oyunun teknik kalitesini ve oyuncunun deneyimini iyileştirir. ⚡

---

## Ray Casting Algoritması 🌍

Cub3d projesindeki 3D labirent görünümünü elde etmenin temel yöntemi **Ray Casting** algoritmasıdır. Bu algoritma sayesinde 2D haritayı, birinci şahıs bakış açısıyla 3D görünüme dönüştürebiliriz.

**Ray Casting** prensipleri:
1. Oyuncu, oyun dünyası içerisinde belirli bir konuma (x, y) ve bakış açısına (angle) sahiptir.
2. Ekranın her dikey “pixel sütunu” için, oyuncunun bakış açısına göre bir “ışın” (ray) gönderilir.
3. Işın, harita üzerinde duvar veya engel ile kesişene kadar uzatılır.
4. Işının duvarla ilk kesiştiği nokta, o sütunda çizilecek olan duvar yüksekliğini belirler.
5. Her sütun için duvar yüksekliği hesaplanarak, ekranda 3D bir perspektif algısı oluşturulur.

Aşağıda örnek bir **C** fonksiyon kodu, basit bir ray casting mantığının iskeletini göstermektedir:

```c
#include <math.h>
#include <stdio.h>

#define FOV 60.0      // Oyuncunun görüş açısı (derece)
#define SCREEN_WIDTH 640
#define SCREEN_HEIGHT 480

// Harita tanımı (0 -> boş alan, 1 -> duvar)
int map[8][8] = {
    {1,1,1,1,1,1,1,1},
    {1,0,0,0,0,0,0,1},
    {1,0,1,0,0,0,0,1},
    {1,0,0,0,0,1,0,1},
    {1,0,0,0,0,0,0,1},
    {1,0,0,1,0,0,0,1},
    {1,0,0,0,0,0,0,1},
    {1,1,1,1,1,1,1,1},
};

// Oyuncu başlangıç konumu
float playerX = 3.5, playerY = 3.5;
float playerAngle = 0.0;

void cast_rays(void)
{
    // Ekrandaki her piksel sütunu için bir ışın hesapla
    for (int x = 0; x < SCREEN_WIDTH; x++) {
        // Ekrandaki x sütunu, -FOV/2 ile +FOV/2 arasındaki açılara haritalanır
        float rayAngle = (playerAngle - (FOV / 2.0)) + ((float)x / SCREEN_WIDTH) * FOV;
        
        // Dereceyi radyana çevir
        float rayRad = rayAngle * M_PI / 180.0;

        // Işın izleme (ray marching)
        float stepSize = 0.1;  // Işın adım boyu
        float distance = 0.0;  // Duvara olan mesafe
        float hitX = playerX;
        float hitY = playerY;
        
        int hitWall = 0;

        // Harita sonuna kadar veya duvar bulana kadar ışını ilerlet
        while (!hitWall && distance < 20.0) {  // 20 => maksimum görüş mesafesi gibi
            distance += stepSize;
            hitX = playerX + cos(rayRad) * distance;
            hitY = playerY + sin(rayRad) * distance;

            // Harita sınırı içinde mi
            if (hitX < 0 || hitX >= 8 || hitY < 0 || hitY >= 8) {
                hitWall = 1;
                distance = 20.0;  // Duvar bulunmuş gibi kabul et
            } else if (map[(int)hitY][(int)hitX] == 1) {
                hitWall = 1;  // Duvarı bulduk
            }
        }

        // Duvar yüksekliğini hesapla (basit yaklaşım)
        int wallHeight = (int)(SCREEN_HEIGHT / distance);
        
        // Bu sütunda duvarı çizebilirsiniz:
        // draw_vertical_line(x, wallHeight); // Örn. kullanıcıya özel bir çizim fonksiyonu
    }
}
```
Bu örnek, ray casting mantığını çok basite indirgenmiş şekilde anlatmaktadır. Gerçekte, duvar çizimi ve texture mapping gibi ek işlemler yaparak daha detaylı bir görüntü elde edebilirsiniz.

## Tools Used 🛠️

Cub3d projesinde kullanılan bazı önemli araçlar ve kütüphaneler şunlardır:

- **MiniLibX:** Grafik işlemleri ve pencere yönetimi için kullanılan bir kütüphanedir.
- **Xlib:** X Window System ile etkileşim kurmak için kullanılan bir kütüphanedir.

## Requirements 📋

Cub3d projesini çalıştırmak için aşağıdaki gereksinimlerin karşılanması gerekir:

- Unix tabanlı bir işletim sistemi (Linux, macOS)
- GCC derleyicisi
- MiniLibX kütüphanesi
