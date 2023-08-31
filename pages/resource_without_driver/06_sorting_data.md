## Sorting board games

<v-clicks>

```php{all|10-14|13|9}
final class BoardGameGrid extends AbstractGrid implements ResourceAwareGridInterface
{
    // [...]

    public function buildGrid(GridBuilderInterface $gridBuilder): void
    {
        $gridBuilder
            ->setProvider(BoardGameGridProvider::class)
            ->orderBy('name', 'asc')
            ->addField(
                StringField::create('name')
                    ->setLabel('Name')
                    ->setSortable(true),
            )
            // [...]
        ;
    }

    // [...]
}

```

</v-clicks>

---
layout: image
image: /sorting_board_games_01.png
transition: fade
---

---
layout: image
image: /sorting_board_games_02.png
transition: fade
---

---
transition: fade
---

```php{all|9|11|11,16-26}
final class BoardGameGridProvider implements DataProviderInterface
{
    public function getData(Grid $grid, Parameters $parameters): Pagerfanta
    {
        $data = [];

        $fileData = $this->getFileData();

        $sorting = $parameters->get('sorting') ?? [];

        $fileData = $this->sortData($fileData, $sorting);
        
        // [...]
    }

    private function sortData(array $data, array $sorting): array
    {
        if ('asc' === ($sorting['name'] ?? null)) {
            usort($data, [$this, 'sortByNameAsc']);
        }

        if ('desc' === ($sorting['name'] ?? null)) {
            usort($data, [$this, 'sortByNameDesc']);
        }

        return $data;
    }
}

```

---

```php{all|7-9|7-9,18-21|11-13|11-13,23-26}
final class BoardGameGridProvider implements DataProviderInterface
{
    // [...]
    
    private function sortData(array $data, array $sorting): array
    {
        if ('asc' === ($sorting['name'] ?? null)) {
            usort($data, [$this, 'sortByNameAsc']);
        }

        if ('desc' === ($sorting['name'] ?? null)) {
            usort($data, [$this, 'sortByNameDesc']);
        }

        return $data;
    }

    private function sortByNameAsc($a, $b): int
    {
        return strcmp($a[1], $b[1]);
    }

    private function sortByNameDesc($a, $b): int
    {
        return strcmp($b[1], $a[1]);
    }
}

```

---
layout: image
image: /sorting_board_games_03.png
transition: fade
---

---
layout: image
image: /sorting_board_games_04.png
transition: fade
---

---

## Default sorting

Is it sorted by name?

---
layout: image
image: /sorting_board_games_05.png
transition: fade
---

---
transition: fade
---

```php{all|9}
final class BoardGameGridProvider implements DataProviderInterface
{
    public function getData(Grid $grid, Parameters $parameters): Pagerfanta
    {
        $data = [];

        $fileData = $this->getFileData();

        $sorting = $parameters->get('sorting') ?? [];

        $fileData = $this->sortData($fileData, $sorting);
        
        // [...]
    }

    // [...]
}

```

---

```php{9}
final class BoardGameGridProvider implements DataProviderInterface
{
    public function getData(Grid $grid, Parameters $parameters): Pagerfanta
    {
        $data = [];

        $fileData = $this->getFileData();

        $sorting = $parameters->get('sorting') ?? $grid->getSorting();

        $fileData = $this->sortData($fileData, $sorting);
        
        // [...]
    }

    // [...]
}

```

---
layout: image
image: /sorting_board_games_06.png
transition: fade
---
