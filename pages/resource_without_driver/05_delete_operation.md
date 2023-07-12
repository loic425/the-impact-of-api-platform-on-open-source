## Deleting board games

<v-clicks>

```php{all|9|9,6|10|10,5|11|11,4}
namespace App\BoardGameBlog\Infrastructure\Sylius\Resource;

// [...]
use App\BoardGameBlog\Infrastructure\Sylius\State\Processor\DeleteBoardGameProcessor;
use App\BoardGameBlog\Infrastructure\Sylius\State\Provider\BoardGameItemProvider;
use Sylius\Component\Resource\Metadata\Delete;

// [...]
#[Delete(
    provider: BoardGameItemProvider::class,
    processor: DeleteBoardGameProcessor::class,
)]
final class BoardGameResource implements ResourceInterface
{
    // [...]
}

```

</v-clicks>

---

```php {all|4|4,2|6|8|10|10,15-26|12}
// [...]
use Sylius\Component\Resource\State\ProcessorInterface;

final class DeleteBoardGameProcessor implements ProcessorInterface
{
    public function process(mixed $data, Operation $operation, Context $context): mixed
    {
        Assert::isInstanceOf($data, BoardGameResource::class);

        $this->deleteBoardGame($data);

        return null;
    }

    private function deleteBoardGame(BoardGameResource $boardGameResource): void
    {
        $fileData = $this->getFileData();
        unset($fileData[$boardGameResource->id]);

        $handle = fopen($this->dataDir . '/board_games.csv', 'wb');

        foreach ($fileData as $data) {
            fputcsv($handle, $data);
        }

        fclose($handle);
    }
}

```

---
layout: image
image: /removing_board_game_01.png
transition: fade
---

---
layout: image
image: /removing_board_game_02.png
transition: fade
---

---
layout: image
image: /removing_board_game_03.png
transition: fade
---
