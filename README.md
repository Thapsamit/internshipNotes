## how to add foreign key in mirgations file exmaple

```js 
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.addColumn('Posts', 'userId', {
      type: Sequelize.INTEGER,
      references: {
        model: 'Users', // Target table name
        key: 'id', // Target column name
      },
      onUpdate: 'CASCADE',
      onDelete: 'SET NULL', // or 'CASCADE' or other options
    });
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.removeColumn('Posts', 'userId');
  },
};

```

### Undo migrations files

```bash
npx sequelize-cli db:migrate:undo
```
