## Removing many books

<v-clicks>

We'll use `Bulk Delete` operation which allows to remove several items of your resource at the same time.

```php {all|15|15,4}
namespace App\Entity;

use App\Form\BookType;
use Sylius\Component\Resource\Metadata\BulkDelete;
use Sylius\Component\Resource\Metadata\Index;
use Sylius\Component\Resource\Metadata\Create;
use Sylius\Component\Resource\Metadata\Delete;
use Sylius\Component\Resource\Metadata\Resource;
use Sylius\Component\Resource\Metadata\Update;
use Sylius\Component\Resource\Model\ResourceInterface;

#[Resource(formType: BookType::class)]
#[Create]
#[Update]
#[BulkDelete]
#[Delete]
#[Index]
class Book implements ResourceInterface
{
}

```

</v-clicks>

---

## Route

<v-clicks>

It will configure this route for your `bulk_delete` operation.

| Name                 | Method       | Path               |
|----------------------|--------------|--------------------|
| app_book_bulk_delete | DELETE, POST | /books/bulk_delete |    


</v-clicks>

---

```php {all|15-19|15|16|17}
final class BookGrid extends AbstractGrid implements ResourceAwareGridInterface
{
    // [...]

    public function buildGrid(GridBuilderInterface $gridBuilder): void
    {
        $gridBuilder
            // [...]
            ->addActionGroup(
                ItemActionGroup::create(
                    UpdateAction::create(),
                    DeleteAction::create(),
                )
            )
            ->addActionGroup(
                BulkActionGroup::create(
                    DeleteAction::create()
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
image: /removing_many_books_01.png
transition: fade
---

---
layout: image
image: /removing_many_books_02.png
transition: fade
---

---
layout: image
image: /removing_many_books_03.png
transition: fade
---

---
layout: image
image: /removing_many_books_04.png
transition: fade
---
