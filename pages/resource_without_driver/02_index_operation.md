## Browsing board games

<v-clicks>

```php{all|9-13|9-13,3}
namespace App\BoardGameBlog\Infrastructure\Sylius\Resource;

use Sylius\Component\Resource\Metadata\Index;
use Sylius\Component\Resource\Metadata\Resource;
use Sylius\Component\Resource\Model\ResourceInterface;
use Symfony\Component\Validator\Constraints\NotBlank;

#[Resource]
#[Index(
    grid: 'app_board_game'
)]
final class BoardGameResource implements ResourceInterface
{
    // [...]
}

```

</v-clicks>

---

```php {all|22-25|10}
// src/BoardGameBlog/Infrastructure/Sylius/Grid/BoardGameGrid.php

final class BoardGameGrid extends AbstractGrid implements ResourceAwareGridInterface
{
    public static function getName(): string { return 'app_board_game'; }

    public function buildGrid(GridBuilderInterface $gridBuilder): void
    {
        $gridBuilder
            ->setProvider(BoardGameGridProvider::class)
            ->addField(
                StringField::create('name')
                    ->setLabel('Name')
            )
            ->addField(
                StringField::create('shortDescription')
                    ->setLabel('Short Description'),
            )
        ;
    }

    public function getResourceClass(): string
    {
        return BoardGameResource::class;
    }
}

```

---
transition: fade
---

```php{all|6|7,5|7,5,11|13|15|16|18-22|25}

namespace App\BoardGameBlog\Infrastructure\Sylius\Grid\DataProvider;

// [...]
use Sylius\Component\Grid\Data\DataProviderInterface;

final class BoardGameGridProvider implements DataProviderInterface
{
    public function __construct(private readonly string $dataDir) {}

    public function getData(Grid $grid, Parameters $parameters): Pagerfanta
    {
        $data = [];

        foreach ($this->getFileData() as $row) {
            [$id, $name, $shortDescription] = $row;

            $data[] = new BoardGameResource(
                id: $id,
                name: $name,
                shortDescription: $shortDescription,
            );
        }

        return new Pagerfanta(new ArrayAdapter($data));
    }
}

```

---

```php{all|10-13}
namespace App\BoardGameBlog\Infrastructure\Sylius\Grid\DataProvider;

// [...]
use Sylius\Component\Grid\Data\DataProviderInterface;

final class BoardGameGridProvider implements DataProviderInterface
{
    // [...]

    private function getFileData(): array
    {
        return array_map('str_getcsv', file($this->dataDir . '/board_games.csv'));
    }
}

```

---
layout: image
image: /browsing_board_games_01.png
transition: fade
---

---
layout: image
image: /browsing_board_games_02.png
transition: fade
---
