# Тестовое задание: Простой редактор сметы
## Описание задачи
Создать простое приложение для редактирования строительной сметы с автосохранением и уведомлениями о сохранении.

## Технические требования

- Frontend: Next.js 15+, React 19, TypeScript
- Styling: Styled-components (как в основном проекте) или Ant Design
- State Management: Redux Toolkit (уже используется в проекте)
- Формы: React Hook Form + Zod валидация
- Данные: Redux store + localStorage для персистентности

## Функциональные требования
1. Основной функционал (обязательно)

Таблица позиций: отображение сметы в виде таблицы
- Добавление позиции: простая форма с полями:

  - Наименование
  - Количество
  - Цена за единицу

- Редактирование: клик на ячейку для редактирования
- Удаление: кнопка удаления строки
- Автоподсчет: итоговая сумма обновляется автоматически

2. Автосохранение и уведомления

- Автосохранение: при каждом изменении данные сохраняются в localStorage
- Уведомления о сохранении: показать toast "Смета сохранена" после изменений
- Debounce: использовать задержку ~500мс чтобы не спамить уведомлениями
- Статус сохранения: опционально показать индикатор "Сохранено" / "Сохранение..."

3. Next.js структура

- Одна страница: `/` - главная страница с редактором сметы
- Один API route: `GET /api/health` - простая проверка что API работает
- Middleware: логирование запросов (опционально)
- Можно добавить страницу `/about` для демонстрации роутинга

4. Архитектура

- Redux Toolkit: store для управления состоянием сметы
- Персистентность: автосохранение в localStorage при изменениях
- Формы: React Hook Form для добавления/редактирования позиций

## Структура данных
```typescript
interface EstimateItem {
id: string;
name: string;
quantity: number;
pricePerUnit: number;
totalPrice: number; // quantity * pricePerUnit
}
interface Estimate {
id: string;
items: EstimateItem[];
totalSum: number;
}
```
Рекомендуемый стек (на основе нашего проекта)
```json
{
"dependencies": {
"@reduxjs/toolkit": "^2.5.0",
"react-hook-form": "^7.52.1",
"react-redux": "^9.2.0",
"styled-components": "^6.1.13",
"zod": "^3.23.8",
"uuid": "^11.1.0",
"classnames": "^2.5.1"
}
}
```
Или можно использовать Ant Design для быстрой разработки UI:
```json
{
"dependencies": {
"antd": "^5.22.5",
"@ant-design/nextjs-registry": "^1.0.2"
}
}
```
## UI требования

- Простая таблица с инпутами для редактирования
- Кнопка "Добавить позицию"
- Отображение общей суммы
- Стили: styled-components или Ant Design компоненты
- Валидация через Zod схемы (числа > 0, обязательные поля)

- Автосохранение с уведомлениями
```typescript
// Пример логики с debounce
import { debounce } from 'lodash.debounce';
const debouncedSave = debounce((data: Estimate) => {
localStorage.setItem('estimate', JSON.stringify(data));
toast.success('Смета сохранена');
}, 500);
// В Redux middleware или useEffect
useEffect(() => {
if (estimate.items.length > 0) {
debouncedSave(estimate);
}
}, [estimate]);
```
### Возможные улучшения (абсолютно по желанию или настроению, в зависимости от свободного времени):

- Статус сохранения: индикатор где-то в интерфейсе "Сохранение..." / "Сохранено"
- Восстановление данных: загрузка сохраненной сметы при перезагрузке страницы
- Уведомления разных типов: success для сохранения, error для ошибок валидации
- Экспорт: кнопка "Скачать JSON" текущей сметы
