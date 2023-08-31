## Publishing books

<v-clicks>

We'll use `apply_state_machine_transition` operation which allows to apply a transition using a state machine.

```php {all|9|9,4}
namespace App\Entity;

use App\Form\BookType;
use Sylius\Component\Resource\Metadata\ApplyStateMachineTransition;
use Sylius\Component\Resource\Model\ResourceInterface;

#[Resource(formType: BookType::class)]
// [...]
#[ApplyStateMachineTransition(stateMachineTransition: 'publish')]
class Book implements ResourceInterface
{
}

```

</v-clicks>

---

## Route

<v-clicks>

It will configure this route for your `apply_state_machine_transition` operation.

| Name              | Method     | Path                |
|-------------------|------------|---------------------|
| app_book_publish  | PUT, PATCH | /books/{id}/publish |      


</v-clicks>

---

```php {all|14-19|15|16|17}
final class BookGrid extends AbstractGrid implements ResourceAwareGridInterface
{
    // [...]

    public function buildGrid(GridBuilderInterface $gridBuilder): void
    {
        $gridBuilder
            // [...]
            ->addField(
                StringField::create('author')
                    ->setLabel('sylius.ui.author')
                    ->setSortable(true)
            )
            ->addField(
                TwigField::create('state', '@SyliusUi/Grid/Field/state.html.twig')
                    ->setLabel('sylius.ui.state')
                    ->setOption('vars', ['labels' => 'admin/book/label/state']),
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

```twig
<!-- templates/admin/book/label/draft.html.twig -->
<span class="ui blue label">
    <i class="inbox icon"></i>
    {{ value|trans }}
</span>

```


---
layout: image
image: /publishing_book_01.png
transition: fade
---

---

```php {all|12-21|12|13|14|15|16-18|20|21}
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
                    ApplyTransitionAction::create(
                        name: 'publish',
                        route: 'app_admin_book_publish',
                        routeParameters: ['id' => 'resource.id'],
                        options: [
                            'class' => 'green',
                        ],
                    )
                        ->setLabel('app.ui.publish')
                        ->setIcon('checkmark'),
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
image: /publishing_book_02.png
transition: fade
---

---
layout: image
image: /publishing_book_03.png
transition: fade
---

---
layout: image
image: /publishing_book_04.png
transition: fade
---
