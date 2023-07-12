## Removing books

<v-clicks>

We'll use `Delete` operation which allows to remove an existing item of your resource.

```php {all|13|13,5}
namespace App\Entity;

use Sylius\Component\Resource\Metadata\Index;
use Sylius\Component\Resource\Metadata\Create;
use Sylius\Component\Resource\Metadata\Delete;
use Sylius\Component\Resource\Metadata\Resource;
use Sylius\Component\Resource\Metadata\Update;
use Sylius\Component\Resource\Model\ResourceInterface;

#[Resource]
#[Create]
#[Update]
#[Delete]
#[Index]
class Book implements ResourceInterface
{
}

```

</v-clicks>

## Route

<v-clicks>

It will configure this route for your `delete` operation.

| Name            | Method | Path        |
|-----------------|--------|-------------|
| app_book_delete | DELETE | /books/{id} |


</v-clicks>

---

```php {all|14-19|16|17}
final class BookGrid extends AbstractGrid implements ResourceAwareGridInterface
{
    // [...]

    public function buildGrid(GridBuilderInterface $gridBuilder): void
    {
        $gridBuilder
            // [...]
            ->addActionGroup(
                MainActionGroup::create(
                    CreateAction::create(),
                )
            )
            ->addActionGroup(
                ItemActionGroup::create(
                    UpdateAction::create(),
                    DeleteAction::create(),
                )
            )
        ;
    }

    public function getResourceClass(): string
    {
        return Book::class;
    }
}

```

---
layout: image
image: /removing_book_01.png
transition: fade
---

---
layout: image
image: /removing_book_02.png
transition: fade
---

---
layout: image
image: /removing_book_03.png
transition: fade
---
