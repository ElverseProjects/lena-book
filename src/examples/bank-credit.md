# Bank credit lab

Пример микро-проекта на Lena: банковские кредиты, динамический массив записей, добавление, поиск и фильтрация.

```lena
#=
  @file bank_credit.le
  @brief Lab project: Bank credit registry.
=#

credit = struct {
    account_number:i32,
    client_last_name:str,
    client_first_name:str,
    amount:f64,
    percent:i32,
}

read_credit():credit {
    let item:credit = {}

    item.account_number = cli::in("Account number: "):i32
    item.client_last_name = cli::in("Client last name: "):str
    item.client_first_name = cli::in("Client first name: "):str
    item.amount = cli::in("Credit amount: "):f64
    item.percent = cli::in("Credit percent: "):i32

    return item
}

read_credits(count:u64):credit[*] {
    let credits:credit[*] = credit[*]::alloc(count)

    for i = 0...count by 1 {
        cli::out("\nClient {@i + 1}\n")

        let item:credit = read_credit()

        credits.push(item) else err {
            cli::err("Cannot add credit record: {@err}\n")
            return credits
        }
    }

    return credits
}

add_new_credit(credits:credit[*]):nil! {
    cli::out("\nAdd new credit record\n")

    let item:credit = read_credit()

    credits.push(item)
}

print_credit(const item:credit):nil {
    cli::out(
        "{@item.account_number} | {@item.client_last_name} | {@item.client_first_name} | {@item.amount} | {@item.percent}\n"
    )
}

print_credits(const credits:credit[*]):nil {
    cli::out("+------------+----------------+----------------+--------------+---------+\n")
    cli::out("| Account    | Last name      | First name     | Amount       | Percent |\n")
    cli::out("+------------+----------------+----------------+--------------+---------+\n")

    for item = credits by 1 {
        print_credit(item)
    }

    cli::out("+------------+----------------+----------------+--------------+---------+\n")
}

is_same_client(
    const item:credit,
    const last_name:str,
    const first_name:str
):bool {
    return str::eq(item.client_last_name, last_name) and
           str::eq(item.client_first_name, first_name)
}

print_client_credits_and_sum(
    const credits:credit[*],
    const last_name:str,
    const first_name:str
):f64 {
    let total:f64 = 0.0
    let found:bool = false

    cli::out("\nCredits for {@last_name} {@first_name}:\n")

    for item = credits by 1 {
        if (is_same_client(item, last_name, first_name)) {
            print_credit(item)
            total = total + item.amount
            found = true
        }
    }

    if (not found) {
        cli::out("No credits found for this client.\n")
    }

    return total
}

find_by_percent(const credits:credit[*], target_percent:i32):credit[*] {
    let result:credit[*] = credit[*]::alloc(0)

    for item = credits by 1 {
        if (item.percent == target_percent) {
            result.push(item) else err {
                cli::err("Cannot save filtered result: {@err}\n")
                return result
            }
        }
    }

    return result
}

main(argc:i32, const argv:str[]):exit_status {
    cli::out("Bank credit registry\n")

    let count:u64 = cli::in("Enter number of clients: "):u64

    let credits:credit[*] = read_credits(count)

    cli::out("\nAll credits:\n")
    print_credits(credits)

    add_new_credit(credits) else err {
        cli::err("Cannot add new credit: {@err}\n")
        return ::FAILURE
    }

    cli::out("\nAfter adding new credit:\n")
    print_credits(credits)

    let search_last_name:str = cli::in("\nEnter client last name: "):str
    let search_first_name:str = cli::in("Enter client first name: "):str

    let total:f64 = print_client_credits_and_sum(
        credits,
        search_last_name,
        search_first_name
    )

    cli::out(
        "\nTotal credit amount for {@search_last_name} {@search_first_name}: {@total}\n"
    )

    let target_percent:i32 = cli::in("\nEnter credit percent to filter: "):i32

    let filtered:credit[*] = find_by_percent(credits, target_percent)

    cli::out("\nClients with percent {@target_percent}:\n")
    print_credits(filtered)

    return ::SUCCESS
}
```

## Что здесь удобно

- Данные описаны как struct, без ручных `malloc` для каждой записи.
- `credit[*]` — динамический массив с понятными методами.
- `const credits:credit[*]` явно говорит, что функция только читает массив.
- `push` и `resize` возвращают fallible operation, а не C-style `bool`/`NULL`.
- `for item = credits by 1` не требует ручного индекса.

## Что ещё не решено

- Полная модель ownership для `str`.
- Форматирование таблицы с шириной колонок.
- Должен ли `str::eq` стать оператором `==`.
