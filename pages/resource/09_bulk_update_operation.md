## Publishing many books

<v-clicks>

We'll use `Bulk Update` operation which allows to update several items of your resource at the same time.

```php {all|9|9,4|10|11|12}
namespace App\Entity;

use App\State\Processor\PublishBookProcessor;
use Sylius\Component\Resource\Metadata\BulkUpdate;
use Sylius\Component\Resource\Model\ResourceInterface;

#[Resource]
// [...]
#[BulkUpdate(
    shortName: 'bulk_publish',
    processor: PublishBookProcessor::class,
    validate: false,
)]
class Book implements ResourceInterface
{
}

```

</v-clicks>

---

## Route

<v-clicks>

It will configure this route for your `bulk_publish` operation.

| Name                  | Method     | Path                |
|-----------------------|------------|---------------------|
| app_book_bulk_publish | PUT, PATCH | /books/bulk_publish |    


</v-clicks>

---

```php {all|12-24|12|15-21|16-18}
final class BookGrid extends AbstractGrid implements ResourceAwareGridInterface
{
    // [...]

    public function buildGrid(GridBuilderInterface $gridBuilder): void
    {
        $gridBuilder
            // [...]
            ->addActionGroup(
                BulkActionGroup::create(
                    DeleteAction::create(),
                    Action::create('publish', 'apply_transition')
                        ->setLabel('app.ui.publish')
                        ->setIcon('icon: checkmark')
                        ->setOptions([
                            'link' => [
                                'route' => 'app_admin_book_bulk_publish',
                            ],
                            'class' => 'green',
                        ]),
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
image: /publishing_many_books_01.png
transition: fade
---

---
layout: image
image: /publishing_many_books_02.png
transition: fade
---

---
layout: image
image: /publishing_many_books_03.png
transition: fade
---

---
layout: image
image: /publishing_many_books_04.png
transition: fade
---
