# What's next?

<v-clicks>

* Stable release
* Remove Kernel events
* PHP configuration

</v-clicks>

---

### Kernel Events

<img alt="Kernel events" src="/kernel_events.png">

---

<img alt="Kernel events" src="/remove_event_listeners.png">

---

## PHP configuration

Open-source cooperation

<div align="center">
<img class="w-75" align="center" src="https://sylius.com/wp-content/uploads/2021/03/sylius-logo_sylius-logo-light-1024x422.jpg">
</div>

<div align="center">
<img class="w-75" src="https://api-platform.com/logo.png">
</div>

---

### Resource configurator

```php
// config/package/sylius_resource.php
namespace Symfony\Component\DependencyInjection\Loader\Configurator;

use App\Entity\Book;
use App\Entity\Subscription;
use Sylius\Component\Loader\Configuration\ResourceConfigurator;

return static function (ResourceConfigurator $resourceConfigurator): void {
    $resourceConfigurator
        ->withResource((new Resource(Book::class))
            ->withRoutePrefix('admin')
            ->withOperations([
                new Index(),
                new Create(),
                new Update(),
                new Delete(),    
            ])
        )
        ->withResource((new Resource(Subscription::class))
            ->withOperations([
                new Index(),
            ])
        )
    ;
};
```

---
layout: image
image: https://www.lelabocoworking.com/wp-content/uploads/2019/07/Coop%C3%A9ration.jpg
transition: fade
---