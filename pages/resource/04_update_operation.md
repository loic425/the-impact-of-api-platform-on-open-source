## Editing books

<v-clicks>

We'll use `Update` operation which allows to edit an existing item of your resource.

```php {all|12|12,7}
namespace App\Entity;

use App\Form\BookType;
use Sylius\Component\Resource\Metadata\Index;
use Sylius\Component\Resource\Metadata\Create;
use Sylius\Component\Resource\Metadata\Resource;
use Sylius\Component\Resource\Metadata\Update;
use Sylius\Component\Resource\Model\ResourceInterface;

#[Resource(formType: BookType::class)]
#[Create]
#[Update]
#[Index]
class Book implements ResourceInterface
{
}

```

</v-clicks>

---

## Route

<v-clicks>

It will configure this route for your `update` operation.

| Name            | Method                | Path             |
|-----------------|-----------------------|------------------|
| app_book_update | GET, PUT, PATCH, POST | /books/{id}/edit |


</v-clicks>

---

```php {all|14-18|14|15|16}
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
image: /editing_book_01.png
transition: fade
---

---
layout: image
image: /editing_book_02.png
transition: fade
---

---
layout: image
image: /editing_book_03.png
transition: fade
---

---
layout: image
image: /editing_book_04.png
transition: fade
---
