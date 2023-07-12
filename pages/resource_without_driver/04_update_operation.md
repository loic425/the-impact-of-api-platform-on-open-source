## Editing board games

<v-clicks>

```php{all|9|9,6|10|10,5|11|11,4}
namespace App\BoardGameBlog\Infrastructure\Sylius\Resource;

// [...]
use App\BoardGameBlog\Infrastructure\Sylius\State\Processor\UpdateBoardGameProcessor;
use App\BoardGameBlog\Infrastructure\Sylius\State\Provider\BoardGameItemProvider;
use Sylius\Component\Resource\Metadata\Update;

// [...]
#[Update(
    provider: BoardGameItemProvider::class,
    processor: UpdateBoardGameProcessor::class,
)]
final class BoardGameResource implements ResourceInterface
{
    // [...]
}

```

</v-clicks>

---

```php {all|4|4,2|8|10-11|13-14|16|18-20|22-26}
// [...]
use Sylius\Component\Resource\State\ProviderInterface;

final class BoardGameItemProvider implements ProviderInterface
{
    public function __construct(private readonly string $dataDir) {}

    public function provide(Operation $operation, Context $context): ?BoardGameResource
    {
        $request = $context->get(RequestOption::class)?->request();
        Assert::notNull($request);

        $id = $request->attributes->get('id');
        Assert::nullOrString($id);

        [$id, $name, $shortDescription] = $this->getFileData()[$id] ?? null;

        if (null === $id) {
            return null;
        }

        return new BoardGameResource(
            id: $id,
            name: $name,
            shortDescription: $shortDescription,
        );
    }
}

```

---

```php {all|4|4,2|6|8|10|10,15-26|12}
// [...]
use Sylius\Component\Resource\State\ProcessorInterface;

final class UpdateBoardGameProcessor implements ProcessorInterface
{
    public function process(mixed $data, Operation $operation, Context $context): mixed
    {
        Assert::isInstanceOf($data, BoardGameResource::class);

        $this->updateBoardGame($data);

        return null;
    }

    private function updateBoardGame(BoardGameResource $boardGameResource): void
    {
        $fileData = $this->getFileData();
        $row = &$fileData[$boardGameResource->id];

        $row[1] = $boardGameResource->name;
        $row[2] = $boardGameResource->shortDescription;

        $handle = fopen($this->dataDir . '/board_games.csv', 'wb');

        foreach ($fileData as $data) {
            fputcsv($handle, $data);
        }

        fclose($handle);
    }

    private function getFileData(): array
    {
        return array_column(
            array_map(
                'str_getcsv',
                file($this->dataDir . '/board_games.csv')
            ),
            null,
            0,
        );
    }
}

```

---
layout: image
image: /editing_board_game_01.png
transition: fade
---

---
layout: image
image: /editing_board_game_02.png
transition: fade
---

---
layout: image
image: /editing_board_game_03.png
transition: fade
---

---
layout: image
image: /editing_board_game_04.png
transition: fade
---
