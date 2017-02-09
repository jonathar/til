# How to Create has_and_belongs_to_many factories

## Setting up the factories

```
factory :comment do
  # attributes
end

factory :post do
  comments {[FactoryGirl.create(:comment)]}
end
```

## Utilizing the factories
```
comment = create(:comment)
user = create(:post, :comments => [comment])
```

[source](http://stackoverflow.com/questions/1484374/how-to-create-has-and-belongs-to-many-associations-in-factory-girl)
