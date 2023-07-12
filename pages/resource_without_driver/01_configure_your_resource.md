```php{all|7|7,3|8|8,4|8,4,17-20}
namespace App\BoardGameBlog\Infrastructure\Sylius\Resource;

use Sylius\Component\Resource\Metadata\Resource;
use Sylius\Component\Resource\Model\ResourceInterface;
use Symfony\Component\Validator\Constraints\NotBlank;

#[Resource]
final class BoardGameResource implements ResourceInterface
{
    public function __construct(
        public ?string $id = null,
        #[NotBlank] public ?string $name = null,
        public ?string $shortDescription = null,
    ) {
    }

    public function getId(): string
    {
        return $this->id;
    }
}

```
