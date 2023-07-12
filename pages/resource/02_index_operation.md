## Browsing books

<v-clicks>

We'll use `Index` operation which allows to browse all items of your resource.

```php {all|8|8,3}
namespace App\Entity;

use Sylius\Component\Resource\Metadata\Index;
use Sylius\Component\Resource\Metadata\Resource;
use Sylius\Component\Resource\Model\ResourceInterface;

#[Resource]
#[Index]
class Book implements ResourceInterface
{
}

```

</v-clicks>

---

## Route

<v-clicks>

It will configure this route for your `index` operation.

| Name                  | Method          | Path    |
|-----------------------|-----------------|---------|
| app_book_index        | GET             | /books  |


</v-clicks>

---

## Create a grid

You can use the Grid maker.

```bash
$ bin/console make:grid
```

---

```php {all|1|3-6|11|12-16|17-21}
final class BookGrid extends AbstractGrid implements ResourceAwareGridInterface
{
    public static function getName(): string
    {
        return 'app_book';
    }

    public function buildGrid(GridBuilderInterface $gridBuilder): void
    {
        $gridBuilder
            ->orderBy('name', 'asc')
            ->addField(
                StringField::create('name')
                    ->setLabel('sylius.ui.name')
                    ->setSortable(true)
            )
            ->addField(
                StringField::create('author')
                    ->setLabel('sylius.ui.author')
                    ->setSortable(true)
            )
        ;    
    }
```

---

## Use this grid for your index operation

<v-clicks>

To use a grid for you operation, you need to install the [Sylius grid package](https://github.com/Sylius/SyliusGridBundle/)

```php {all|9-10|9-10,3|11-12|11-12}
namespace App\Entity;

use App\Grid\BookGrid;
use Sylius\Component\Resource\Metadata\Index;
use Sylius\Component\Resource\Metadata\Resource;
use Sylius\Component\Resource\Model\ResourceInterface;

#[Resource]
// You can use either the FQCN of your grid
#[Index(grid: BookGrid::class)]
// Or you can use the grid name
#[Index(grid: 'app_book')]
class Book implements ResourceInterface
{
}

```

</v-clicks>

---
layout: image
image: /browsing_books_01.png
---
