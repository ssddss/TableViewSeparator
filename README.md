# TableViewSeparator
设置分隔的线的三种方法
1.自定义cell,手动添加分割线

隐藏自带的
tableView.separatorStyle = UITableViewCellSeparatorStyleNone;

可以通过addSubview的方式添加一条分割线；也可以自绘分割线。

// 自绘分割线
- (void)drawRect:(CGRect)rect
{
    CGContextRef context = UIGraphicsGetCurrentContext();

    CGContextSetFillColorWithColor(context, [UIColor whiteColor].CGColor);
    CGContextFillRect(context, rect);

    CGContextSetStrokeColorWithColor(context, [UIColor colorWithRed:0xE2/255.0f green:0xE2/255.0f blue:0xE2/255.0f alpha:1].CGColor);
    CGContextStrokeRect(context, CGRectMake(0, rect.size.height - 1, rect.size.width, 1));
}
2.重写cell的setFrame方法,高度-1,露出背景色

- (void)setFrame:(CGRect)frame
{
    frame.size.height -= 1;
    // 给cellframe赋值
    [super setFrame:frame];
}
取消系统的分割线
设置tableView背景色为分割线颜色
// 取消系统分割线
self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
// 设置tableView背景色
self.tableView.backgroundColor = [UIColor colorWithWhite:215 / 255.0 alpha:1];
3.利用系统属性设置(separatorInset, layoutMargins)共需添加三句代码:

对tableView的separatorInset, layoutMargins属性的设置
-(void)viewDidLoad {
  [super viewDidLoad];
  //1.调整(iOS7以上)表格分隔线边距
  if ([self.tableView respondsToSelector:@selector(setSeparatorInset:)]) {
      self.tableView.separatorInset = UIEdgeInsetsZero;
  }
  //2.调整(iOS8以上)view边距(或者在cell中设置preservesSuperviewLayoutMargins,二者等效)
  if ([self.tableView respondsToSelector:@selector(setLayoutMargins:)]) {
      self.tableView.layoutMargins = UIEdgeInsetsZero;
  }
}
对cell的LayoutMargins属性的设置
对cell的设置可以写在cellForRowAtIndexPath里,也可以写在willDisplayCell方法里

-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *ID = @"cell";
    FSDiscoverSpecialCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];
    if (cell == nil) {
        cell = [[FSDiscoverSpecialCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:ID];
    }

   //2.调整(iOS8以上)tableView边距(与上面第2步等效,二选一即可)
    if ([cell respondsToSelector:@selector(setPreservesSuperviewLayoutMargins:)]) {
        cell.preservesSuperviewLayoutMargins = NO;
    }
   //3.调整(iOS8以上)view边距
    if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {
        [cell setLayoutMargins:UIEdgeInsetsZero];
    }
    return cell;
}
