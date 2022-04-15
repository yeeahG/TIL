### Data frame 만들기
hflight2.groupby("Dest")[['Cancelled']].sum()

- value값만 나옴
hflight2.groupby("Dest")['Cancelled'].sum()
