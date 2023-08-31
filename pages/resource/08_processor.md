## Publishing books with custom processor

<v-clicks>

We configure an `update` operation.

```php {all|8-13|8-13,4|9|10|11|11,3|12}
namespace App\Entity;

use App\State\Processor\PublishBookProcessor;
use Sylius\Component\Resource\Metadata\Update;
use Sylius\Component\Resource\Model\ResourceInterface;

// [...]
#[Update(
    methods: ['PUT', 'PATCH'],
    shortName: 'publish',
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

It will configure this route for your `publish` operation.

| Name              | Method     | Path                |
|-------------------|------------|---------------------|
| app_book_publish  | PUT, PATCH | /books/{id}/publish |      


</v-clicks>


---

```php{all|9|9,6|12|12,7|13|13,4|17|19|20|23}
// src/State/Processor/PublishBookProcessor.php

use Sylius\Component\Resource\Context\Context;
use Sylius\Component\Resource\Doctrine\Common\State\PersistProcessor;
use Sylius\Component\Resource\Metadata\Operation;
use Sylius\Component\Resource\State\ProcessorInterface;
use Symfony\Component\Workflow\WorkflowInterface;

final class PublishBookProcessor implements ProcessorInterface
{
    public function __construct(
        private readonly WorkflowInterface $bookPublishingStateMachine,
        private readonly PersistProcessor $persistProcessor,
    ) {
    }

    public function process(mixed $data, Operation $operation, Context $context): mixed
    {
        if ($this->bookPublishingStateMachine->can($data, 'publish')) {
            $this->bookPublishingStateMachine->apply($data, 'publish');
        }

        return $this->persistProcessor->process($data, $operation, $context);
    }
}

```

---
layout: image
image: /publishing_book_04.png
transition: fade
---

---
layout: image
image: /publishing_book_05.png
transition: fade
---

---
layout: image
image: /publishing_book_06.png
transition: fade
---
