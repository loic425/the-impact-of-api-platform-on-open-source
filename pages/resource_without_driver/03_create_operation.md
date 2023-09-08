## Adding board games

<v-clicks>

```php{all|11|11,5|17|17,4}
namespace App\BoardGameBlog\Infrastructure\Sylius\Resource;

// [...]
use App\BoardGameBlog\Infrastructure\Sylius\State\Processor\CreateBoardGameProcessor;
use App\BoardGameBlog\Infrastructure\Symfony\Form\Type\BoardGameType;
use Sylius\Component\Resource\Metadata\Index;
use Sylius\Component\Resource\Metadata\Create;

#[Resource(
    driver: false, 
    formType: BoardGameType::class),
]
#[Index(
    grid: 'app_board_game'
)]
#[Create(
    processor: CreateBoardGameProcessor::class,
)]
final class BoardGameResource implements ResourceInterface
{
    // [...]
}

```

</v-clicks>

---

```php {all|6|6,4|6,4,10|12|14|14,19-25|21|23|24|16}
namespace App\BoardGameBlog\Infrastructure\Sylius\State\Processor;

// [...]
use Sylius\Component\Resource\State\ProcessorInterface;

final class CreateBoardGameProcessor implements ProcessorInterface
{
    public function __construct(private readonly string $dataDir) {}

    public function process(mixed $data, Operation $operation, Context $context): mixed
    {
        Assert::isInstanceOf($data, BoardGameResource::class);

        $this->createBoardGame($data);

        return null;
    }

    private function createBoardGame(BoardGameResource $boardGameResource): void
    {
        $handle = fopen($this->dataDir . '/board_games.csv', 'a');
        
        fputcsv($handle, [(string) Uuid::v4(), $boardGameResource->name, $boardGameResource->shortDescription]);
        fclose($handle);
    }
}


```

---
layout: image
image: /adding_board_game_01.png
transition: fade
---

---
layout: image
image: /adding_board_game_02.png
transition: fade
---

---
layout: image
image: /adding_board_game_03.png
transition: fade
---

---
layout: image
image: /adding_board_game_04.png
transition: fade
---

---
layout: image
image: /adding_board_game_05.png
transition: fade
---

---
layout: image
image: /adding_board_game_06.png
transition: fade
---
