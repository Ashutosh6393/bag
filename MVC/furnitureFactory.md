using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
 
namespace dotnet_furniture_factory
{
    public class FurnitureOrder : IFurnitureOrder
    {
        private List<(Furniture furniture, int count)> order = new List<(Furniture furniture, int count)>();
 
        public void addToOrder(Furniture type, int count)
        {
            //implement this method
            order.Add((type, count));
        }
 
        public List<(Furniture, int)> getOrderedFurniture()
        {
            //implement this method
            var data = order;
            return data;
        }
 
        public int getTypeCount(Furniture type)
        {
            //implement this method
            int count = 0;
            var data = order.Where(x => x.furniture == type);
            foreach (var furniture in data)
            {
                count += furniture.count;
            }
            return count;
        }
 
        public float getTypeCost(Furniture type)
        {
            //implement this method
            float cost = 0.0f;
            var data = order.Where(x => x.furniture == type);
            foreach(var furniture in data)
            {
                cost = furniture.furniture.Cost * furniture.count;
            }
            return cost;
        }
 
        public float getTotalOrderCost()
        {
            //implement this method
            float total = 0.0f;
            if (order.Count > 0)
            {
                foreach (var item in order)
                {
                    total += item.furniture.Cost * item.count;
                }
                return total;
            }
            return 0.0f;
        }
 
        public int getTotalOrderQuantity()
        {
            //implement this method
            int count = 0;
            foreach(var item in order)
            {
                count += item.count;
            }
            return count;
        }
    }
}

 
 